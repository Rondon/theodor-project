
var siteTracking = {
  init : function(){
    siteTracking.trackData  = {};
    siteTracking.trackData.path      = siteTracking.getPathUrl();
    if(siteTracking.trackData.path.fullPath.indexOf('createthumb') > -1){
      return;
    }
    var uuid = (navigator.cookieEnabled) ? siteTracking.getCookie('tracking_uuid') : '';
    siteTracking.trackData.uuid      = (uuid != '' && uuid.length == 36) ? uuid : siteTracking.generateUUID();
    siteTracking.setCookie('tracking_uuid', siteTracking.trackData.uuid, 5 * 365);
    siteTracking.trackData.appName   = navigator.appName;
    siteTracking.trackData.userAgent = navigator.userAgent;
    siteTracking.trackData.platform  = navigator.platform;
    siteTracking.trackData.device    = 'Computer';
    if(/Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(siteTracking.trackData.userAgent)){
      siteTracking.trackData.device  = 'Mobile';
    }
    siteTracking.findBrowserAndVersion();
    siteTracking.saveData();
  },

  findBrowserAndVersion : function(){
    if((verOffset = siteTracking.trackData.userAgent.indexOf('Opera')) !=- 1){
      siteTracking.trackData.browserName    = 'Opera';
      siteTracking.trackData.browserVersion = siteTracking.trackData.userAgent.substring(verOffset + 6);
      if((verOffset = siteTracking.trackData.userAgent.indexOf('Version')) != -1)
        siteTracking.trackData.browserVersion = siteTracking.trackData.userAgent.substring(verOffset + 8);
    }

    else if((verOffset = siteTracking.trackData.userAgent.indexOf('OPR')) !=- 1){
      siteTracking.trackData.browserName    = 'Opera';
      siteTracking.trackData.browserVersion = siteTracking.trackData.userAgent.substring(verOffset + 4);
    }

    else if((verOffset = siteTracking.trackData.userAgent.indexOf('MSIE')) != -1){
      siteTracking.trackData.browserName    = 'Internet Explorer';
      siteTracking.trackData.browserVersion = siteTracking.trackData.userAgent.substring(verOffset + 5);
    }

    else if((verOffset = siteTracking.trackData.userAgent.indexOf('Trident/')) != -1){
      siteTracking.trackData.browserName    = 'Internet Explorer';
      var rv = siteTracking.trackData.userAgent.indexOf('rv:');
      siteTracking.trackData.browserVersion = siteTracking.trackData.userAgent.substring(rv + 3, siteTracking.trackData.userAgent.indexOf(')', rv));
    }

    else if((verOffset = siteTracking.trackData.userAgent.indexOf('Edge/')) != -1){
      siteTracking.trackData.browserName    = 'Edge';
      siteTracking.trackData.browserVersion = siteTracking.trackData.userAgent.substring(verOffset + 5);
    }

    else if((verOffset = siteTracking.trackData.userAgent.indexOf('Chrome')) != -1){
      siteTracking.trackData.browserName    = 'Chrome';
      siteTracking.trackData.browserVersion = siteTracking.trackData.userAgent.substring(verOffset + 7, siteTracking.trackData.userAgent.indexOf(' ', verOffset));
    }

    else if((verOffset = siteTracking.trackData.userAgent.indexOf('Safari')) != -1){
      siteTracking.trackData.browserName    = 'Safari';
      siteTracking.trackData.browserVersion = siteTracking.trackData.userAgent.substring(verOffset + 7);
      if((verOffset = siteTracking.trackData.userAgent.indexOf('Version')) != -1)
        siteTracking.trackData.browserVersion = siteTracking.trackData.userAgent.substring(verOffset + 8, siteTracking.trackData.userAgent.indexOf(' ', verOffset));
    }

    else if((verOffset = siteTracking.trackData.userAgent.indexOf('Firefox')) != -1){
      siteTracking.trackData.browserName    = 'Firefox';
      siteTracking.trackData.browserVersion = siteTracking.trackData.userAgent.substring(verOffset + 8);
    }

    else if((nameOffset = siteTracking.trackData.userAgent.lastIndexOf(' ') + 1) < (verOffset = siteTracking.trackData.userAgent.lastIndexOf('/'))){
      siteTracking.trackData.browserName    = siteTracking.trackData.userAgent.substring(nameOffset, verOffset);
      siteTracking.trackData.browserVersion = siteTracking.trackData.userAgent.substring(verOffset + 1);
      if(siteTracking.trackData.browserName.toLowerCase() == siteTracking.trackData.browserName.toUpperCase()){
        siteTracking.trackData.browserName = siteTracking.trackData.appName;
      }
    }
    else{
      siteTracking.trackData.browserName    = '-';
      siteTracking.trackData.browserVersion = '-';
    }

    if((ix = siteTracking.trackData.browserVersion.indexOf(';')) != -1)
      siteTracking.trackData.browserVersion = siteTracking.trackData.browserVersion.substring(0, ix);
    if((ix = siteTracking.trackData.browserVersion.indexOf(' ')) != -1)
      siteTracking.trackData.browserVersion = siteTracking.trackData.browserVersion.substring(0, ix);

    if(siteTracking.trackData.browserVersion.indexOf(")'") > -1){
      siteTracking.trackData.browserVersion = siteTracking.trackData.browserVersion.split(")'").join('')
    }
  },

  saveData : function(){
    $.ajax({
      url  : siteTracking.internalApi.tracking_add_url,
      type : 'post',
      data : {
        delay    : 0,
        qName    : 'tracking',
        attempts : 3,
        remove   : true,
        priority : 'low',
        jobData  : {
          title     : 'User Tracking',
          trackData : {
            uuid           : siteTracking.trackData.uuid,
            path           : siteTracking.trackData.path,
            browserName    : siteTracking.trackData.browserName,
            browserVersion : siteTracking.trackData.browserVersion,
            device         : siteTracking.trackData.device,
            platform       : siteTracking.trackData.platform
          },
          userData : {
            userId : (siteTracking.userId) ? siteTracking.userId : null
          }
        }

      },
      success: function(result){

      }
    });
  },

  getPathUrl : function(){
    var basePath = window.location.origin;
    var domain   = window.location.host;
    var fullPath = window.location.href;
    if(fullPath.length > 1){
      while(fullPath[fullPath.length - 1] == '/'){
        fullPath = fullPath.substring(0, fullPath.length - 1);
      }
    }
    return {domain : domain, basePath : basePath, fullPath : fullPath};
  },

  generateUUID : function(){
    var d = new Date().getTime();
    if(typeof performance !== 'undefined' && typeof performance.now === 'function'){
      d += performance.now();
    }
    return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function (c){
      var r = (d + Math.random() * 16) % 16 | 0;
      d     = Math.floor(d / 16);
      return (c === 'x' ? r : (r & 0x3 | 0x8)).toString(16);
    });
  },

  setCookie : function(cName, cValue, expDays){
    var d = new Date();
    d.setTime(d.getTime() + (expDays * 24 * 60 * 60 * 1000));
    var expires     = 'expires=' + d.toUTCString();
    document.cookie = cName + '=' + cValue + ';' + expires + ';path=/';
  },

  getCookie : function(cName){
    var name          = cName + '=';
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca            = decodedCookie.split(';');
    for(var i = 0; i < ca.length; i++){
      var c = ca[i];
      while(c.charAt(0) == ' '){
        c = c.substring(1);
      }
      if(c.indexOf(name) == 0){
        return c.substring(name.length, c.length);
      }
    }
    return '';
  },

};

$(document).ready(function(){
  if(siteTracking.internalApi)
    siteTracking.init();
});
$(window).on('hashchange', function(e){
  if(siteTracking.internalApi)
    siteTracking.init();
});
