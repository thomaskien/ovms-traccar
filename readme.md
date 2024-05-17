Connecting ovms to traccar is easy using the osmand protocol
====

thanks to @dexterbg https://www.goingelectric.de/forum/viewtopic.php?p=2161887#p2161887

https://docs.openvehicles.com/en/latest/userguide/events.html

<pre>
<code>
// add the following to your /store/scripts/ovmsmain.js and change id and server-url


//every 10 seconds if car is on
function ticker10(){
var on = OvmsMetrics.Value("v.e.on");
var soc = OvmsMetrics.AsFloat("v.b.soc");
var lat = OvmsMetrics.AsFloat("v.p.latitude");
var long = OvmsMetrics.AsFloat("v.p.longitude");
var alt = OvmsMetrics.AsFloat("v.p.altitude");
var speed = OvmsMetrics.AsFloat("v.p.speed")/1.852; //km/h to kn
if (on === true) {
HTTP.Request({ url: "http://TRACCAR-SERVER:5055/?id=1234567&batt="+soc+"&lat="+lat+"&lon="+long+"&alt="+alt+"&speed="+speed});
} 
};


//every 60 minutes if car is off
function ticker3600(){
var on = OvmsMetrics.Value("v.e.on");
var soc = OvmsMetrics.AsFloat("v.b.soc");
var lat = OvmsMetrics.AsFloat("v.p.latitude");
var long = OvmsMetrics.AsFloat("v.p.longitude");
var alt = OvmsMetrics.AsFloat("v.p.altitude");
var speed = OvmsMetrics.AsFloat("v.p.speed")/1.852; //km/h to kn
if (on === false) {
HTTP.Request({ url: "http://TRACCAR-SERVER:5055/?id=1234567&batt="+soc+"&lat="+lat+"&lon="+long+"&alt="+alt+"&speed="+speed});
} 
};

//or use other ticker see ovms-documentation
PubSub.subscribe("ticker.10", ticker10);
PubSub.subscribe("ticker.3600", ticker3600);
</code>
</pre>
