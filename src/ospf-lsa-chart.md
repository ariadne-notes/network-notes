<style>
  .ospf-table {
    border-collapse: collapse;
    width: 100%;
    font-size: 0.9em;
  }
  .ospf-table th,
  .ospf-table td {
    border: 1px solid var(--table-border-color, #ccc);
    padding: 6px 10px;
    vertical-align: top;
  }
  .ospf-table thead th {
    background-color: var(--sidebar-bg, #444);
    color: var(--sidebar-fg, #fff);
    text-align: center;
  }
  .ospf-table td.area-name {
    font-weight: bold;
    white-space: nowrap;
  }
  .ospf-table td.std {
    white-space: nowrap;
    color: var(--fg);
    opacity: 0.65;
    font-size: 0.85em;
  }
  .ospf-table td.config code {
    white-space: nowrap;
  }
  .ospf-table td.auto-inject {
    text-align: center;
  }
  .lsa-cell {
    padding: 0 !important;
    min-width: 60px;
  }
  .lsa-block {
    display: flex;
    flex-direction: column;
  }
  .lsa-box {
    height: 20px;
  }
  .lsa-box.allowed  { background-color: #6dbf6d; }
  .lsa-box.blocked  { background-color: #d94f4f; }
  .lsa-notes {
    padding: 4px 6px;
    font-size: 0.82em;
    color: var(--fg);
  }
  .lsa-notes code {
    font-size: 0.9em;
  }
  .ospf-table tr.area-row td {
    border-top: 2px solid var(--table-border-color, #888);
  }
</style>

<table class="ospf-table">
  <thead>
    <tr>
      <th>Area Type</th>
      <th>Std</th>
      <th>Characteristics</th>
      <th>Config</th>
      <th>Auto Inject Default?</th>
      <th>Type 1</th>
      <th>Type 2</th>
      <th>Type 3</th>
      <th>Type 4</th>
      <th>Type 5</th>
      <th>Type 7</th>
    </tr>
  </thead>
  <tbody>

    <!-- Backbone -->
    <tr class="area-row">
      <td class="area-name">Backbone</td>
      <td class="std">RFC 2328</td>
      <td>
        Needs to be contiguous.<br>
        Can go between areas via a virtual-link.
      </td>
      <td class="config"><code>area 0</code></td>
      <td class="auto-inject">No</td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"><code>default-information originate [always]</code><br>Creates an E2 route.<br>Router becomes an ASBR.</div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box blocked"></div>
        <div class="lsa-notes"></div>
      </div></td>
    </tr>

    <!-- Regular Area -->
    <tr class="area-row">
      <td class="area-name">Regular Area</td>
      <td class="std">RFC 2328</td>
      <td>Needs to be connected to area 0 (can be a virtual link).</td>
      <td class="config"><code>area 1</code></td>
      <td class="auto-inject">No</td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box blocked"></div>
        <div class="lsa-notes"></div>
      </div></td>
    </tr>

    <!-- Stub -->
    <tr class="area-row">
      <td class="area-name">Stub</td>
      <td class="std">RFC 2328</td>
      <td>
        No LSA-5s (External to OSPF routes).<br>
        No routes toward ASBRs (Type 4).
      </td>
      <td class="config"><code>area 2 stub</code></td>
      <td class="auto-inject">Yes</td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes">Default is O IA LSA-3.</div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box blocked"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box blocked"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box blocked"></div>
        <div class="lsa-notes"></div>
      </div></td>
    </tr>

    <!-- Totally Stubby -->
    <tr class="area-row">
      <td class="area-name">Totally Stubby</td>
      <td class="std">Cisco</td>
      <td>
        One LSA-3 for the default route.<br>
        No other LSA-3.
      </td>
      <td class="config"><code>area 2 stub no-summary</code></td>
      <td class="auto-inject">Yes</td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box blocked"></div>
        <div class="lsa-notes">ABR gets <code>no-summary</code>.<br>Automatically generates a default route.</div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box blocked"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box blocked"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box blocked"></div>
        <div class="lsa-notes"></div>
      </div></td>
    </tr>

    <!-- NSSA -->
    <tr class="area-row">
      <td class="area-name">NSSA</td>
      <td class="std">RFC 3101</td>
      <td>
        No LSA-5s (External to OSPF routes).<br>
        Allows inter-area routes (O IA) LSA-3.<br>
        Allows injected external routes via LSA-7.<br>
        ABR translates LSA-5 to LSA-7.
      </td>
      <td class="config"><code>area 2 nssa</code></td>
      <td class="auto-inject">No</td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box blocked"></div>
        <div class="lsa-notes">Uses N-Bit (NSSA).<br>Uses P-BIT (propagate).<br>Default route injectable by ABR.<br><code>area 3 nssa default-information-originate</code></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box blocked"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes">Default is LSA-7.</div>
      </div></td>
    </tr>

    <!-- Totally Stubby NSSA -->
    <tr class="area-row">
      <td class="area-name">Totally Stubby NSSA</td>
      <td class="std">Cisco</td>
      <td>
        No inter-area routes (LSA type 3).<br>
        No routes to ASBRs (LSA type 4).<br>
        No external routes from other parts of the AS (LSA type 5).<br>
        Allows some external routes via LSA type 7.
      </td>
      <td class="config"><code>area 2 nssa no-summary</code></td>
      <td class="auto-inject">No</td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box blocked"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box blocked"></div>
        <div class="lsa-notes"></div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box blocked"></div>
        <div class="lsa-notes">ABR gets <code>no-summary</code>.<br>Automatically generates a default route.</div>
      </div></td>
      <td class="lsa-cell"><div class="lsa-block">
        <div class="lsa-box allowed"></div>
        <div class="lsa-notes"></div>
      </div></td>
    </tr>

  </tbody>
</table>

<p><em>Source: <a href="https://www.cisco.com/c/en/us/support/docs/ip/open-shortest-path-first-ospf/6208-nssa.html">Cisco — OSPF NSSA</a></em></p>
