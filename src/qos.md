### Type of Service
How these 8 bits get used has changed over the years.

<pre>
                   0 1 2 3 4 5 6 7 
                  ┌─────┬─────┬─┬─┐
   RFC 791 (1981) │IP Pr│ ToS │0│0│
                  └─────┴─────┴─┴─┘
                                   
                   0 1 2 3 4 5 6 7 
                  ┌─────┬───────┬─┐
  RFC 1349 (1992) │IP Pr│  TOS  │0│
                  └─────┴───────┴─┘
                                   
                   0 1 2 3 4 5 6 7 
                  ┌───────────┬─┬─┐
  RFC 2474 (1998) │    DSCP   │0│0│
                  └───────────┴─┴─┘
                                   
                   0 1 2 3 4 5 6 7 
                  ┌───────────┬───┐
  RFC 3168 (2001) │    DSCP   │ECN│
                  └───────────┴───┘
</pre>



### QoS Consequences

LAN QoS with voice (buffer management)

 * One voice packet, no voice, but modem will retrain
 * Two voice packets, audio clip, fax call disconnection.
 * VoIP QoS cannot be fixed by adding bandwidth. You simply cannot drop these
 * packets.
 
#### QoS Commands

| Command | Description |
| ------------------------------------ | ------------------------------------------------------------------------- |
| `show mls qos interface f0/0`        | shows if the interface trusts the markings                                |
| `mls qos trust device cisco-phone`   | trusts the phone on the attached port. Uses CDP to verify its a phone     |

### Assured Forwarding
[Assured Forwarding PHB Group](https://datatracker.ietf.org/doc/html/rfc2597)

Used for RED, or WRED.

Four AF classes, each should get it's own resources.

<pre>
                         Class 1        Class 2        Class 3       Class 4    
                    ┌──────────────┬──────────────┬──────────────┬─────────────┐
   Low Drop Prec    │ AF11  001010 │ AF21  010010 │ AF31  011010 │ AF41 100010 │
   Medium Drop Prec │ AF12  001100 │ AF22  010100 │ AF32  011100 │ AF42 100100 │
   High Drop Prec   │ AF13  001110 │ AF23  010110 │ AF33  011110 │ AF43 100110 │
                    └──────────────┴──────────────┴──────────────┴─────────────┘
</pre>