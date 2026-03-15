# network-security-monitoring-firewall


<dashboard version="1.1" theme="dark">
  <label>Firewall monitoring</label>
  <row>
    <panel>
      <title>Blocked Connections</title>
      <chart>
        <search>
          <query>index="botsv2" sourcetype="pan:traffic" action IN (deny, drop, blocked) | stats count by src_port, dest_port, src_ip, dest_ip | sort - count</query>
          <earliest>0</earliest>
          <sampleRatio>1</sampleRatio>
        </search>
        <option name="charting.chart">pie</option>
      </chart>
    </panel>
    <panel>
      <title>Allowed Connections</title>
      <single>
        <search>
          <query>index="botsv2" sourcetype="pan:traffic" action=allow | stats count(action)</query>
          <earliest>0</earliest>
          <sampleRatio>1</sampleRatio>
        </search>
      </single>
    </panel>
    <panel>
      <title>External Source IPs</title>
      <chart>
        <search>
          <query>index="botsv2" sourcetype="pan:traffic" | where NOT cidrmatch("10.0.0.0/8", src_ip)  AND NOT cidrmatch("172.16.0.0/12", src_ip)  AND NOT cidrmatch("192.168.0.0/16", src_ip) | stats count by src_ip, dest_ip, dest_port | sort - count</query>
          <earliest>0</earliest>
          <sampleRatio>1</sampleRatio>
        </search>
        <option name="charting.chart">line</option>
        <option name="charting.drilldown">none</option>
      </chart>
    </panel>
    <panel>
      <title>External Destination IPs</title>
      <chart>
        <search>
          <query>index="botsv2" sourcetype="pan:traffic"
| where NOT cidrmatch("10.0.0.0/8", dest_ip) 
    AND NOT cidrmatch("172.16.0.0/12", dest_ip) 
    AND NOT cidrmatch("192.168.0.0/16", dest_ip)
| stats count by src_ip, dest_ip, dest_port
| sort - count</query>
          <earliest>0</earliest>
          <sampleRatio>1</sampleRatio>
        </search>
        <option name="charting.chart">bar</option>
        <option name="charting.drilldown">all</option>
      </chart>
    </panel>
    <panel>
      <title>Network Traffic by Action (Logarithmic Scale)</title>
      <chart>
        <search>
          <query>index="botsv2" sourcetype="pan:traffic" | timechart span=5m count by action </query>
          <earliest>0</earliest>
          <sampleRatio>1</sampleRatio>
        </search>
        <option name="charting.chart">area</option>
      </chart>
    </panel>
    <panel>
      <title>Traffic by Protocol</title>
      <chart>
        <search>
          <query>index="botsv2" sourcetype="pan:traffic" | timechart span=5m count by protocol </query>
          <earliest>0</earliest>
          <sampleRatio>1</sampleRatio>
        </search>
        <option name="charting.chart">area</option>
      </chart>
    </panel>
  </row>
  <row>
    <panel>
      <title>Traffic by Application / Protocol</title>
      <chart>
        <search>
          <query>index="botsv2" sourcetype="pan:traffic" | timechart span=5m count by app </query>
          <earliest>0</earliest>
          <sampleRatio>1</sampleRatio>
        </search>
        <option name="charting.chart">pie</option>
      </chart>
    </panel>
    <panel>
      <title>Blocked Incoming Traffic by Destination Port</title>
      <single>
        <search>
          <query>index="botsv2" sourcetype="pan:traffic" action IN (deny, drop, blocked) | stats count by src_ip, dest_ip, dest_port | sort - count </query>
          <earliest>0</earliest>
          <sampleRatio>1</sampleRatio>
        </search>
      </single>
    </panel>
    <panel>
      <title>Incoming Traffic by Application / Protocol</title>
      <chart>
        <search>
          <query>index="botsv2" sourcetype="pan:traffic" | table src dest app protocol | head 20 </query>
          <earliest>0</earliest>
          <sampleRatio>1</sampleRatio>
        </search>
        <option name="charting.chart">line</option>
        <option name="charting.drilldown">none</option>
      </chart>
    </panel>
    <panel>
      <title>Top Countries by Blocked Connections</title>
      <chart>
        <search>
          <query>index="botsv2" sourcetype="pan:traffic" action=allowed | iplocation src | stats count by src Country | sort - count | head 10 </query>
          <earliest>0</earliest>
          <sampleRatio>1</sampleRatio>
        </search>
        <option name="charting.chart">line</option>
      </chart>
    </panel>
    <panel>
      <title>Top Blocked Traffic Categories</title>
      <chart>
        <search>
          <query>index="botsv2" sourcetype="pan:threat" | stats count by category| sort - count| head 10</query>
          <earliest>0</earliest>
          <sampleRatio>1</sampleRatio>
        </search>
        <option name="charting.chart">area</option>
      </chart>
    </panel>
  </row>
</dashboard>
