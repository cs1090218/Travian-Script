// ==UserScript==
// @name          Travian under-attack check
// @include      http://*.travian.*
// ==/UserScript==
//set this to false to make it not freeze the screen when running a check, village hop more probable then 
var plusactive = true;
var freezeWhenCheck = false;
var soundUrl = '';//http://download.wavetlan.com/SVV/Media/HTTP/WAV/Media-Convert/Media-Convert_test2_PCM_Mono_VBR_8SS_48000Hz.wav';
var attacked = false;
var villageName = '';
villageNameA = '';
arrivalTime = '';
fullInfo = '';
//alert('Starting');
//Allow you to run a external script\page when you are under attack. feks you could call your own email script.  
var externalUrl = 'asd';
var externalPostData = 'asdasd';
var runEveryTime = true;
function post(url) {
     GM_xmlhttpRequest({
         method: 'GET',
         url: url,
         onload: function (responseDetails) {
             pulled = document.createElement('div');
             pulled.innerHTML = responseDetails.responseText;
             if(plusactive)
             {
                 var contents = pulled.innerHTML;
                 if(contents.indexOf("attack\"") != -1 || contents.indexOf("attack active\"") != -1)
                 {
                    attacked = true;
                    getAttackInfo(pulled)
                    infobar.innerHTML = '<span style=\"color:#FF0000\"><b>' + villageName + ' Under attack!</b>' + '<embed src=\"' + soundUrl + '\" hidden=true autostart=true loop=false></span>';
                    alert(villageName + " under attack.");
                 }
                 /*
                 infobar.innerHTML = pulled;
                 var vill = villNamePlus(pulled);
                 if(vill.length == 0)
                 {
                     alert("No attack");
                 }
                 else
                 {
                     attacked = true;
                     getAttackInfo(pulled)
                     infobar.innerHTML = '<span style=\"color:#FF0000\"><b>' + villageName + ' Under attack!</b>' + '<embed src=\'\' + soundUrl + \'\' hidden=true autostart=true loop=false></span>';
                     alert(villageName + " under attack.");
                 }
                 */
             }
             else
             {
                 if (checkImg(pulled))
                 {
                     attacked = true;
                     getAttackInfo(pulled)
                     infobar.innerHTML = '<span style=\"color:#FF0000\"><b>' + villageName + ' Under attack!</b>' + '<embed src=\"' + soundUrl + '\" hidden=true autostart=true loop=false></span>';
                     alert(villageName + " under attack.");
                 }
                 /*
                 if (tot == ant)
                 {
                     ant++;
                     last();
                 } else {
                     ant++;
                 }*/
             }
         }
     });
 }
function last()
 {
     var ex = "//a[contains(@href,'newdid')][@class='active_vl']";
     tag = document.evaluate(ex, document, null, XPathResult.UNORDERED_NODE_SNAPSHOT_TYPE, null);
     if (tag.snapshotLength)
     {
         temp = tag.snapshotItem(0) .href.split('?') [1].split('&');
         lastlink = temp[0];
         url = 'http://' + document.domain + '/dorf1.php?' + lastlink;
         alert("last");
         alert(url);
         GM_xmlhttpRequest({
             method: 'GET',
             url: url,
             onload: function (responseDetails)
             {
                 pulled = document.createElement('div');
                 pulled.innerHTML = responseDetails.responseText;
                 FreezeScreen('', false);
                 if (checkImg(pulled))
                 {
                     attacked = true;
                     //alert("Under attack!");
                     getAttackInfo(pulled)
                     infobar.innerHTML = '<span style=\"color:#FF0000\"><b>' + villageName + ' Under attack!</b>' + '<embed src=\'\' + soundUrl + \'\' hidden=true autostart=true loop=false></span>';
                 } else {
                     if (!attacked) {
                         infobar.innerHTML = '<span style=\"color:#00CC00\"><b>No attacks found! lucky you =)</b></span>';
                     }
                 }
             }
         });
     } else if (!attacked) {
         FreezeScreen('', false);
         infobar.innerHTML = '<span style=\"color:#00CC00\"><b>No attacks found! lucky you =)</b></span>';
     } else {
         FreezeScreen('', false);
         infobar.innerHTML = '<span style=\"color:#FF0000\"><b>' + villageName + ' Under attack!</b>' + '<embed src=\'\' + soundUrl + \'\' hidden=true autostart=true loop=false></span>';
     }
     external2(externalUrl, externalPostData, attacked);
 }

function villNamePlus(doc)
{
   var ex = ".//div[contains(@id, 'center')]";
   tag = document.evaluate(ex, doc, null, XPathResult.UNORDERED_NODE_SNAPSHOT_TYPE, null);
   ex = ".//div[contains(@id, 'sidebarAfterContent')]";
   tag = document.evaluate(ex, tag.snapshotItem(0), null, XPathResult.UNORDERED_NODE_SNAPSHOT_TYPE, null);
   ex = ".//div[contains(@id, 'sidebarBoxVillageList')]";
   tag = document.evaluate(ex, tag.snapshotItem(0), null, XPathResult.UNORDERED_NODE_SNAPSHOT_TYPE, null);
   ex = ".//div[contains(@id, 'sidebarBoxInnerBox')]";
   tag = document.evaluate(ex, tag.snapshotItem(0), null, XPathResult.UNORDERED_NODE_SNAPSHOT_TYPE, null);
   ex = ".//div[contains(@id, 'innerBox')]";
   tag = document.evaluate(ex, tag.snapshotItem(0), null, XPathResult.UNORDERED_NODE_SNAPSHOT_TYPE, null);
   ex = ".//li[contains(@class, 'attack')]";
   tag = document.evaluate(ex, tag.snapshotItem(0), null, XPathResult.UNORDERED_NODE_SNAPSHOT_TYPE, null);
   if(tag.snapshotLength == 0)
   {
       return new Array();
   }
   ex = ".//div[contains(@class, 'name')]";
   tag = document.evaluate(ex, tag.snapshotItem(0), null, XPathResult.UNORDERED_NODE_SNAPSHOT_TYPE, null);
   //alert(tag.snapshotItem(0).innerHTML);
   var arr = new Array();
   for(var i = 0; i < tag.snapshotLength; i++)
   {
       arr[i] = tag.snapshotItem(0).innerHTML;
   }

   return arr;
}
function checkImg(doc)
{
   var ex = ".//img[contains(@class,'att1')]";
   tag = document.evaluate(ex, doc, null, XPathResult.UNORDERED_NODE_SNAPSHOT_TYPE, null);
   if (tag.snapshotLength)
   {
       return true;
   } else {
       var ex = ".//img[contains(@class,'att3')]";
       tag = document.evaluate(ex, doc, null, XPathResult.UNORDERED_NODE_SNAPSHOT_TYPE, null);
       if (tag.snapshotLength)
       {
           return true;
       } else {
           return false;
       }
   }
   
}
function addDiv() {
   var div = document.createElement('div');
   div.innerHTML = '<div align=\"center\" id=\"FreezePane\" class=\"FreezePaneOff\">' +
   '<div id=\"InnerFreezePane\" class=\"InnerFreezePane\">Test </div>' +
   '</div>';
   document.body.insertBefore(div, document.body.firstChild);
}
function FreezeScreen(msg, state) {
   scroll(0, 0);
   var outerPane = document.getElementById('FreezePane');
   var innerPane = document.getElementById('InnerFreezePane');
   if (state) {
       if (outerPane) outerPane.className = 'FreezePaneOn';
       if (innerPane) innerPane.innerHTML = msg + '<button id=\"closeFreeze\" >x</button>';
       var button = document.getElementById('closeFreeze');
       button.addEventListener('click', function () {
           FreezeScreen('', false)
       }, true);
   } else {
       if (outerPane) outerPane.className = 'FreezePaneOff';
       if (innerPane) innerPane.innerHTML = '';
   }
}
function addGlobalStyle(css) {
   var head,
   style;
   head = document.getElementsByTagName('head') [0];
   if (!head) {
       return ;
   }
   style = document.createElement('style');
   style.type = 'text/css';
   style.innerHTML = css;
   head.appendChild(style);
}
var styles = '.FreezePaneOff' +
'{' +
'visibility: hidden;' +
'display: none;' +
'position: absolute;' +
'top: -100px;' +
'left: -100px;' +
'}' +
'.FreezePaneOn' +
'{' +
'  position: absolute;' +
'  top: 0px;' +
'  left: 0px;' +
'  visibility: visible;' +
'  display: block;' +
'  width: 100%;' +
'  height: 100%;' +
'  background-color: #666;' +
'  z-index: 999;' +
'  filter:alpha(opacity=85);' +
'  -moz-opacity:0.85;' +
'  padding-top: 20%;' +
'}' +
'.InnerFreezePane' +
'{' +
'  text-align: center;' +
' width: 66%;' +
'  background-color: #171;' +
'  color: White;' +
'  font-size: large;' +
'  border: dashed 2px #111;' +
'  padding: 9px;' +
'}';
var ant = 1;
var tot;
function getVilLink()
{
   var ex = "//a[contains(@href,'newdid')][not(@class='active_vl')]";
   tag = document.evaluate(ex, document, null, XPathResult.UNORDERED_NODE_SNAPSHOT_TYPE, null);
   var link = new Array();
   if (tag.snapshotLength)
   {
       for (var i = 1; i <= tag.snapshotLength; i++)
       {
           temp = tag.snapshotItem(i - 1) .href.split('?');
           link[i - 1] = temp[1];
       }
   }
   link.sort(function () {
       return 0.5 - Math.random()
   });
   return link;
}
function underAttackCheck()
{
   var timer = GM_getValue('check_timer', 0);
   timer = parseInt(timer);
   var now = new Date() .getTime();
   var exptime = (timer + (3 + Math.floor(Math.random() * 3)) * 60 * 1000);
   now = '' + now;
   if (1)
   {
       villageName = '';
       villageNameA = '';
       arrivalTime = '';
       fullInfo = '';
       infobar.innerHTML = '<span style=\"color:#FFCC000\"><b>Checking for attacks: <br> Wait about 5-10 seconds depending on internet/system speed</b></span>';
       GM_setValue('check_timer', now);
       var msg = '<b>Checking for attacks: <br> Wait about 5-10 seconds depending on internet/system speed</b>';
       if (freezeWhenCheck) {
           FreezeScreen(msg, true);
       }
       params = getVilLink();
       tot = params.length;
       ant = 1;
       if (tot == 0)
       {
           params[0] = '';
           //assuming only 1 village
           tot = 1;
       }
       for (var i = 1; i <= params.length; i++)
       {
           url = 'http://' + document.domain + '/dorf1.php?' + params[i - 1];
           post(url);
       }
   }
}
function getActiveVillage(doc)
{
   var ex = ".//li[contains(@class,'attack')]";
   tag = document.evaluate(ex, doc, null, XPathResult.UNORDERED_NODE_SNAPSHOT_TYPE, null);
   ex = ".//div[contains(@class,'name')]";
   tag = document.evaluate(ex, tag.snapshotItem(0), null, XPathResult.UNORDERED_NODE_SNAPSHOT_TYPE, null);
   //ex = ".//div[contains(@class, 'name')]";
   //tag = document.evaluate(ex, tag.snapshotItem(0), null, XPathResult.UNORDERED_NODE_SNAPSHOT_TYPE, null);
   if (tag.snapshotLength)
   {
       return tag.snapshotItem(0).innerHTML;
   } else {
       alert("Error in get Active Village");
       return '';
   }
}
function getArrivalTime(doc)
{
   var ex = ".//div[@id='ltbw0' or @id='ltbw1']";
   tag = document.evaluate(ex, doc, null, XPathResult.UNORDERED_NODE_SNAPSHOT_TYPE, null);
   if (tag.snapshotLength)
   {
       div = tag.snapshotItem(0);
       rows = div.getElementsByTagName('tr');
       for (x = 0; x < rows.length; x++)
       {
           cells = rows[x].getElementsByTagName('td');
           if (cells[0].innerHTML.search('att1') > 0)
           {
               arrival = cells[4].getElementsByTagName('span') [0].innerHTML;
               return (arrival);
           }
       }
   }
}
function external2(url, postData, underAttack)
{
   flag = GM_getValue('underAttackFlag', false);
   if (flag && !underAttack) {
       GM_setValue('underAttackFlag', false);
   }
   if (!flag && underAttack)
   {
       GM_setValue('underAttackFlag', true);
       GM_xmlhttpRequest({
           method: 'POST',
           url: url,
           headers: {
               'Content-type': 'application/x-www-form-urlencoded'
           },
           data: encodeURI(postData),
           onload: function (responseDetails)
           {
               pulled = document.createElement('div');
               pulled.innerHTML = responseDetails.responseText;
           }
       });
   } else if (runEveryTime && underAttack)
   {
       GM_xmlhttpRequest({
           method: 'POST',
           url: url,
           headers: {
               'Content-type': 'application/x-www-form-urlencoded'
           },
           data: encodeURI(postData),
           onload: function (responseDetails)
           {
               pulled = document.createElement('div');
               pulled.innerHTML = responseDetails.responseText;
           }
       });
   }
}
function reload()
{
   url = 'http://google.com';
   GM_xmlhttpRequest({
       method: 'GET',
       url: url,
       onload: function (responseDetails)
       {
           pulled = document.createElement('div');
           pulled.innerHTML = responseDetails.responseText;
           // this reloading method avoids the browser asking whether to submit form again
           if (location.href.indexOf('#') > 0) {
               location.href = location.href.substring(0, location.href.length - 1);
               // remove trailing '#' or reload won't work   
           } 
           else {
               location.href = location.href;
           }
       }
   });
}
function getAttackInfo(code)
{
   if (villageName.length > 0)
   {
       var newVillName = getActiveVillage(code);
       villageName = villageName + ', ' + newVillName;
       arrivalTime = arrivalTime + ', ' + getArrivalTime(code);
       fullInfo = fullInfo + ', ' + villageName + '-' + getArrivalTime(code);
   } else {
       villageName = getActiveVillage(code);
       arrivalTime = '--:--:--';//getArrivalTime(code);
       fullInfo = villageName + '-' + getArrivalTime(code);
   }
}

function underAttackCheckPlus()
{
   villageName = '';
   villageNameA = '';
   arrivalTime = '';
   fullInfo = '';
   infobar.innerHTML = '<span style=\"color:#FFCC000\"><b>Checking for attacks: <br> Wait about 5-10 seconds depending on internet/system speed</b></span>';
   var msg = '<b>Checking for attacks: <br> Wait about 5-10 seconds depending on internet/system speed</b>';
   if (freezeWhenCheck) {
       FreezeScreen(msg, true);
   }
   post('http://' + document.domain + '/dorf1.php');
}

function sendMail() {
   var link = "mailto:shashank.verma590@gmail.com"
            + "&subject=" + escape("This is my subject")
            + "&body=" + escape("this is body")
   ;

   window.location.href = link;
}

var infobar = document.createElement('div');
thisDiv = document.getElementById('center');
thisDiv.appendChild(infobar);
addGlobalStyle(styles);
addDiv();
if(plusactive)
{
   window.addEventListener('load', underAttackCheckPlus, true);
   window.setInterval(function () {
      underAttackCheckPlus()
   }, 50 * 1000);
}
else
{
   window.addEventListener('load', underAttackCheck, true);
   window.setInterval(function () {
     underAttackCheck()
   }, 60 * 1000);
}
window.setInterval(function () {
   reload()
}, 15 * 60 * 1000);
