# Stateful Switchover.

<pre>
┌──────────────────────┐
│┌────────────────────┐│
││ RP-2 (Standby)     ││
│└────────────────────┘│
│    ▲                 │
│    │ State           │
│┌───┴────────────────┐│
││ RP-1 (Active)      ││
│└─┬──────────────────┘│
│  │ BGP            ▲  │
│  ▼            BGP │  │
│┌──────────────────┴─┐│
││ Linecard-1         ││
│└────────────────────┘│
│                      │
└──────────────────────┘
</pre>

The owner of the control plane is the **RP**, the Route Processor.

The actual router connections terminate on the linecard.

With multiple RPs, if one RP has a catastrophic failure, the other RP can take over without dropping traffic.

# Terms
* **RIB:** Routing Information Base. This is where the RP stores its routes.
* **FIB:** Forwarding Information Base. This is the information necessary to program the linecard to pass traffic.
* **SSO:** Stateful Switchover. The RPs sync with each other and share state, (hopefully) enough state to prevent traffic disruption.
* **Checkpointing:** All necessary information to perform the task is already on the standby RP.
* **Non-Stop Routing:** AKA NSR. The Control plane relationships and RIB are both checkpointed.
* **Non-Stop Forwarding:** AKA NSF. The FIB is checkpointed.
* **Graceful Restart:** SSO/NSF/NSR are all vendor features that do no share state with the neighbor. GR is an IETF capability both devices must have turned on.

## Graceful Restart Mechanics
This is a BGP Example.

<pre>
     ┌───────────────────┐                 ┌────────────────┐                  
     │ GR-Capable Router │                 │  GR-Aware Peer │                  
     └───────────────────┘                 └────────┬───────┘                  
              │                                     │                          
              │◄─── OPEN with GR Capability ───────►│                          
 Router       │                                     │                          
 Restart  ───►│                                     │                          
              │                                     │                          
              │                                     │                          
   * Send     │                                     │ * Acknowledge restart    
     Restart  ├─── OPEN with Restart Bit Set ──────►│ * Mark routes stale      
     Notif.   │                                     │ * Start Restart Timer    
              │                                     │                          
              │◄── OPEN with Capability ───────────►│                          
              │                                     │                          
* Session     │                                     │                          
  Established │──────── BGP Hello ─────────────────►│ * Stop Restart Timer     
              │                                     │ * Start Stale-Path Timer 
              │                                     │                          
              │◄──── Send Initial Updates + EoR ────┤                          
              │                                     │                          
    * Best    │                                     │                          
      Path    │                                     │ * Stop Stale-Path Timer  
      Select  │──────── Send Updates + EoR ───────► │ * Delete stale prefixes  
      on EoR  │                                     │ * Refresh with new ones  
              │                                     │                          
              │           *** CONVERGED ***         │                          
</pre>


# References
[Cisco - Introduction to HA Technologies: SSO/NSF with GR and/or NSR)(https://archive.nanog.org/meetings/nanog42/presentations/Weissner_SSO.pdf)

[Graceful Restart Mechanism for BGP](https://datatracker.ietf.org/doc/html/rfc4724)