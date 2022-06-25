> 声明：本页面仅供JS脚本学习、交流和讨论，请勿使用它们进行非法勾当。

**请不要主动向他人宣传本页面**

<hr></hr>

## 腾讯视频
<details><summary>点击查看 (上次更新: 2022/06/21)</summary>

```javascript
try {
    var a = prompt(PLAYER._DownloadMonitor.context.dataset.title, PLAYER._DownloadMonitor.context.dataset.ckc ? PLAYER._DownloadMonitor.context.dataset.currentVideoUrl : PLAYER._DownloadMonitor.context.dataset.currentVideoUrl.replace(/:.*qq.com/g, "://defaultts.tc.qq.com/defaultts.tc.qq.com"));
} catch (error) {
    if(__PLAYER__.currentVideoInfo.config.chachaParam != null) {
        alert("不支持CHACHA加密");
    } else {
    var a = prompt(__PLAYER__.tvplayConfig.playTitle, __PLAYER__.currentVideoInfo.loadingUrl.replace(/:.*qq.com/g, "://defaultts.tc.qq.com/defaultts.tc.qq.com"));
    }
}
```
</details>

## 腾讯视频(WV DRM内容)
<details><summary>点击查看 (上次更新: 2022/06/25)</summary>

```javascript
var download = function (text, filename) {
    let b = new Blob([text], {
      type: 'text/plain'
    });
    let a = document.createElement("a");
    a.href = URL.createObjectURL(b);
    a.setAttribute("download", filename);
    a.click();
};
var vTitle = __PLAYER__.tvplayConfig.playTitle;
var m3u8Content = __PLAYER__.currentVideoInfo.config.m3u8[Object.keys(__PLAYER__.currentVideoInfo.config.m3u8)[0]];
m3u8Content = m3u8Content.replace(/https?:.*?qq.com/g, "https://defaultts.tc.qq.com/defaultts.tc.qq.com");
var mName = vTitle + "[v+a].m3u8";
var licUrl = __PLAYER__.currentVideoInfo.config.drmConfig.licenseUrl;
m3u8Content = m3u8Content.replace("#EXTM3U", "#EXTM3U\n" + "#EXT-WV-LIC-URL:" + licUrl);
download(m3u8Content, mName);
```
</details>

## 腾讯视频(ChaCha20 DRM内容)
<details><summary>点击查看 (上次更新: 2022/06/21)</summary>

```javascript
var importWasmAsync = async function () {
  let jsCode = await fetch("https://vm.gtimg.cn/thumbplayer/txp/dec_video_1.0.0.js").then(resp => resp.text());
  let script = document.createElement("script");
  script.text = jsCode;
  document.getElementsByTagName("head")[0].appendChild(script);
};
var i = function (e) {
  var t = self
    , r = t.Module
    , n = t.HEAPU8
    , i = e.length + 64
    , o = r._malloc(i);
  return r.stringToUTF8Array(e, n, o, i),
    o
};
var chacha20NdAsync = async function () {
  await importWasmAsync();
  await new Promise( r => setTimeout(r, 300));
  var e = __PLAYER__.currentVideoInfo.config.chachaParam;
  var t, r = e.linkvid, o = e.base, a = e.appVer, s = e.tm, l = e.platform;
  var u = i(r)
    , c = i(o)
    , h = i(a);
  _prepareParam(u, c, h, s, l),
    _free(u),
    _free(c),
    _free(h);
  var kB64 = btoa(String.fromCharCode(...new Uint8Array(Module.HEAP8.slice(5243552, 5243552 + 32))));
  var nB64 = btoa(String.fromCharCode(...new Uint8Array(Module.HEAP8.slice(5244608, 5244608 + 8))));
  var _url = __PLAYER__.currentVideoInfo.loadingUrl.replace(/:.*qq.com/g, "://defaultts.tc.qq.com/defaultts.tc.qq.com");
  var a = prompt(__PLAYER__.tvplayConfig.playTitle, JSON.stringify({ URL: _url, ChaCha20KeyBase64: kB64, ChaCha20NonceBase64: nB64 }));
};
chacha20NdAsync();
```
</details>


## 爱奇艺(iqiyi.com)视频
<details><summary>点击查看 (上次更新: 2022/06/06)</summary>

```javascript
var download = function (text, filename, type) {
    let b = new Blob([text], {
      type: type || 'text/plain'
    });
    let a = document.createElement("a");
    a.href = URL.createObjectURL(b);
    a.setAttribute("download", filename);
    a.click();
};
var findPssh = function (base64Str) {
    var ba = Uint8Array.from(atob(base64Str), c => c.charCodeAt(0));
    for(let i = 0, f = false; i < ba.length - 5; i++) {
        if(ba[i+1] == 0x70 && ba[i+2] == 0x73 && ba[i+3] == 0x73 && ba[i+4] == 0x68) {
            if(f) {
                let p_len = ba[i];
                let pssh = ba.slice(i-3, i+p_len-3);
                return btoa(String.fromCharCode.apply(null, pssh));
            }
            f = true;
        }
    }
};
var importCmd5xAsync = async function() {
    let jsCode = await fetch("https://static.iqiyi.com/js/common/f6a3054843de4645b34d205a9f377d25.js").then(resp => resp.text());
    let script = document.createElement("script");
    script.text = jsCode;
    document.getElementsByTagName("head")[0].appendChild(script);
};
var importJsZipAsync = async function() {
    let jsCode  = await fetch("https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.0/jszip.min.js").then(resp => resp.text());
    let script = document.createElement("script");
    script.text = jsCode;
    document.getElementsByTagName("head")[0].appendChild(script);
};
var formatTime = function(dur) {
    var date = new Date(0);
    date.setSeconds(dur);
    return date.toISOString().substr(11, 8).replace(/:/,"h").replace(/:/,"m")+"s";
};
var fetchFlvAsync = async function(info) {
    var fs = info.fs;
    var content = "#EXTM3U\n";
    var prefix = "https://data.video.iqiyi.com/videos";
    let results = fs.map(async fs_i => {
        let url = fs_i.l;
        let  api = prefix + url;
        let t = "";
        if(playerObject._player.package.engine.adproxy.engine.movieinfo.current.boss){
            t = playerObject._player.package.engine.adproxy.engine.movieinfo.current.boss.data.t;
        }
        api = prefix + url + "%E2%9C%97domain=1&t=" + t + "&QY00001=" + /qd_uid=(\d+)/g.exec(url)[1] + "&ib=4&ptime=0&ibt=" + cmd5x(t + /\/(\w{10,})/g.exec(url)[1]);
        let resp = await fetch(api,{credentials: 'same-origin'});
        let json = await resp.json();
        return "#EXTINF:"+(fs_i.d/1000).toFixed(2)+"\n" + json["l"];
    });
    let urls = await Promise.all(results);
    content += urls.join('\n');
    content += "\n#EXT-X-ENDLIST";
    return content;
};
var ndIqyAsync = async function() {
    var vTracks = playerObject._player.package.engine.adproxy.engine.movieinfo.current.vidl.filter(a=>a.isUsable && a.realArea.width).sort((a,b)=>b.vsize-a.vsize);
    let info = vTracks[0];
    if(vTracks.length > 1) {
        let p = "";
        vTracks.forEach(function (item, index) {
            p += `\r\n[${index}]: [${item.realArea.width}x${item.realArea.height}]_${(item.vsize / 1024 / 1024).toFixed(2)}MB`;
        });
        let _input = prompt("请选择视频" + p);
        info = vTracks[Number(_input)];
    }
    var title = "";
    try{
        title = document.querySelector('h1.player-title a.title-link[title]')['title'] + "_" + document.querySelector('h1.player-title em').innerText + "_" + info.realArea.width + "_" + info.realArea.height + "_" + formatTime(info.duration) + "_" + (info.vsize / 1024 / 1024).toFixed(2) + "MB";
    } catch(err){
        title = document.title + "_" + info.realArea.width + "_" + info.realArea.height + "_" + formatTime(info.duration) + "_" + (info.vsize / 1024 / 1024).toFixed(2) + "MB";
    } 
    var m3u8Content = "";
    if(typeof info.playlist == "string" && info.playlist.length>0){
        m3u8Content = info.playlist;
    }else if(typeof info.playlist == "object" && !info.playlist.drm){
        m3u8Content = "#EXTM3U\n" + info.playlist.urls.map((u,i)=>"#EXTINF:"+info.playlist.durations[i]+",\nhttps:"+u+info.playlist.qdp[/\/(\w{10,})/g.exec(u)[1]]).join('\n') + "\n#EXT-X-ENDLIST";
    }else if(typeof info.playlist == "object" && info.playlist.drm){
        let keyServer = info.playlist.drm.keySystemServer;
        let aTrack1 = info.playlist.m3u8.audio_track1;
        let vTrack1 = info.playlist.m3u8.video_track1;
        /* 生成audio m3u8 */
        let aM3u8 = "#EXTM3U\n";
        aM3u8 += `#EXT-X-MAP:URI=\"init.m4a\"\n`;
        aM3u8 += "#EXT-X-KEY:METHOD=PLZ-KEEP-RAW,URI=\"None\"\n";
        let aUrl = aTrack1.files[0].file_name;
        for (let i = 0; i < aTrack1.files[0].seekable.time.length-1; i++) {
            let _start = aTrack1.files[0].seekable.pos[i];
            let _end = aTrack1.files[0].seekable.pos[i+1];
            let _dur = aTrack1.files[0].seekable.time[i+1]-aTrack1.files[0].seekable.time[i];
            aM3u8 += `#EXTINF:${_dur.toFixed(2)},\n`;
            aM3u8 += `${aUrl}&start=${_start}&end=${_end}\n`;
        }
        aM3u8 += "\n#EXT-X-ENDLIST";
        /* 生成video m3u8 */
        let vM3u8 = "#EXTM3U\n";
        vM3u8 += `#EXT-X-MAP:URI=\"init.mp4\"\n`;
        vM3u8 += "#EXT-X-KEY:METHOD=PLZ-KEEP-RAW,URI=\"None\"\n";
        vM3u8 += vTrack1.files.map((u)=>`#EXTINF:${u.duration_second.toFixed(2)},\n${u.file_name}&start=${u.seekable.pos_start}&end=${u.seekable.pos_end}`).join('\n');
        vM3u8 += "\n#EXT-X-ENDLIST";
        /* 引入JSZip */
        await importJsZipAsync();
        /* zip打包 */
        var zip = new JSZip();
        /* 塞入文件 */
        zip.file("init.m4a",aTrack1.codec_init, {base64: true});
        zip.file("init.mp4",vTrack1.codec_init, {base64: true});
        zip.file("m4a.m3u8",aM3u8);
        zip.file("mp4.m3u8",vM3u8);
        zip.file("license_url.txt",keyServer);
        zip.file("pssh.txt",findPssh(vTrack1.codec_init));
        zip.generateAsync({type:"blob"}).then(function(content) {
            download(content, title + ".zip", "application/octet-stream");
        });
    }else{
        await importCmd5xAsync();
        m3u8Content = await fetchFlvAsync(info);
    }
    if(m3u8Content!="") download(m3u8Content, title + ".m3u8");
};
ndIqyAsync();
```
</details>


## 爱奇艺(iqiyi.com)视频 高码率1080P/24fps(650)
<details><summary>点击查看 (上次更新: 2022/06/22)</summary>

```javascript
var download = function (text, filename, type) {
    let b = new Blob([text], {
      type: type || 'text/plain'
    });
    let a = document.createElement("a");
    a.href = URL.createObjectURL(b);
    a.setAttribute("download", filename);
    a.click();
};
var formatTime = function(dur) {
    var date = new Date(0);
    date.setSeconds(dur);
    return date.toISOString().substr(11, 8).replace(/:/,"h").replace(/:/,"m")+"s";
};
var importCmd5xAsync = async function() {
    let jsCode = await fetch("https://static.iqiyi.com/js/common/f6a3054843de4645b34d205a9f377d25.js").then(resp => resp.text());
    let script = document.createElement("script");
    script.text = jsCode;
    document.getElementsByTagName("head")[0].appendChild(script);
};
var getCookie = function (sName) {
    var aCookie = document.cookie.split("; ");
    for (var i = 0; i < aCookie.length; i++) {
        var aCrumb = aCookie[i].split("=");
        if (sName == aCrumb[0])
            return unescape(aCrumb[1]);
    }
    return null;
};
var doRequestAsync = async function(lid, ct, cf) {
    let movieinfo = playerObject._player.package.engine.adproxy.engine.movieinfo;
    let src = "01010031010000000000";
    let host = "https://cache.video.iqiyi.com";
    let params = "/dash?tvid=" + movieinfo.tvid
        + "&bid=800&vid=" + movieinfo.vid
        + "&src=" + src + "&vt=0&rs=1&uid=" + getCookie("P00003")
        + "&ori=pcw&ps=0&k_uid=" + getCookie("QC005")
        + "&pt=0&d=0&s=&lid=" + lid + "&ct=" + ct + "&cf=1&k_tag=1&ost=0&ppt=0&dfp=" + getCookie("__dfp")
        + "&locale=zh_cn&k_err_retries=0&qd_v=2&tm=" + (new Date()).getTime()
        + "&qdy=a&qds=0&k_ft1=740530597658614&k_ft2=2147483647&k_ft6=3859108094"
        + "&fr_600=60__60_60__&fr_860=_____&k_ft4=1732989035757060&agent_type=21"
        + "&sver=2&k_ver=13.5.0&k_ft7=163495&&k_ft5=41&k_ft8=_&k_err_retries=0"
        + "&ut=4&ut=5&ut=14&ut=17&ut=54&pano264=600&pano265=600&pre=0&fv=2";
    let api = host + params + "&vf=" + cmd5x(params);
    return await fetch(api,{credentials: 'include'}).then(resp => resp.json());
};
var ndIqyAsync = async function() {
    await importCmd5xAsync();
    let info = (await doRequestAsync()).data.program.video.sort((a,b)=>b.bid-a.bid)[0];
    let m3u8Content = info.m3u8;
    var title = "";
    try{
        title = document.querySelector('h1.player-title a.title-link[title]')['title'] + "_" + document.querySelector('h1.player-title em').innerText + "_" + info.scrsz + "_" + (info.code == 2 ? "H264" : "H265") + "_" + formatTime(info.duration) + "_" + (info.vsize / 1024 / 1024).toFixed(2) + "MB";
    } catch(err){
        title = document.title + "_" + info.scrsz + "_" + (info.code == 2 ? "H264" : "H265") + "_" + formatTime(info.duration) + "_" + (info.vsize / 1024 / 1024).toFixed(2) + "MB";
    } 
    if(m3u8Content && m3u8Content!="") download(m3u8Content.replaceAll(/http.*?qiyi.com/g,"http://data.video.iqiyi.com"), title + ".m3u8");
    else alert("解析失败!");
};
ndIqyAsync();
```
</details>


## 爱奇艺(iq.com)视频
<details><summary>点击查看 (上次更新: 2022/06/05)</summary>

```javascript
var download = function (text, filename) {
    let b = new Blob([text], {
      type: 'text/plain'
    });
    let a = document.createElement("a");
    a.href = URL.createObjectURL(b);
    a.setAttribute("download", filename);
    a.click();
};
var ndIq = function() {
    var info = playerObject._player.package.engine.adproxy.engine.movieinfo.current.originalData.data.program.video.find(a=>a._selected);
    var m3u8Content = info.m3u8;
    var title = document.querySelector('.intl-album-title span span').innerText.trim() + "_" + document.querySelector('.intl-play-title a').nextSibling.nodeValue + "_" + info.scrsz + "_" + (info.code == 2 ? "H264": "H265") + "_" + document.getElementsByClassName("iqp-time-dur")[0].innerText.replace(/:/, ".") + "_" + (info.vsize / 1024 / 1024).toFixed(2) + "MB.m3u8";
    if(m3u8Content!="") download(m3u8Content, title);
};
ndIq();
```
</details>

## 爱奇艺(iqiyi.com)音轨下载
<details><summary>点击查看 (上次更新: 2022/06/05)</summary>

```javascript
var download = function (text, filename) {
    let b = new Blob([text], {
      type: 'text/plain'
    });
    let a = document.createElement("a");
    a.href = URL.createObjectURL(b);
    a.setAttribute("download", filename);
    a.click();
};
var importCmd5xAsync = async function() {
    let jsCode = await fetch("https://static.iqiyi.com/js/common/f6a3054843de4645b34d205a9f377d25.js").then(resp => resp.text());
    let script = document.createElement("script");
    script.text = jsCode;
    document.getElementsByTagName("head")[0].appendChild(script);
};
var getCookie = function (sName) {
    var aCookie = document.cookie.split("; ");
    for (var i = 0; i < aCookie.length; i++) {
        var aCrumb = aCookie[i].split("=");
        if (sName == aCrumb[0])
            return unescape(aCrumb[1]);
    }
    return null;
};
var doRequestAsync = async function(lid, ct, cf) {
    let movieinfo = playerObject._player.package.engine.adproxy.engine.movieinfo;
    let src = "01010031010000000000";
    let host = "https://cache.video.iqiyi.com";
    let params = "/dash?tvid=" + movieinfo.tvid
        + "&bid=300&vid=" + movieinfo.vid
        + "&src=" + src + "&vt=0&rs=1&uid=" + getCookie("P00003")
        + "&ori=pcw&ps=0&k_uid=" + getCookie("QC005")
        + "&pt=0&d=0&s=&lid=" + lid + "&ct=" + ct + "&cf=" + (cf == "aac" ? "2" : "1") + "&k_tag=1&ost=0&ppt=0&dfp=" + getCookie("__dfp")
        + "&locale=zh_cn&k_err_retries=0&qd_v=2&tm=" + (new Date()).getTime()
        + "&qdy=a&qds=0&k_ft1=740531601218477&k_ft4=1162183859249156&k_ft5=1&ut=1&ut=5";
    let api = host + params + "&vf=" + cmd5x(params);
    return await fetch(api,{credentials: 'include'}).then(resp => resp.json());
};
var ndIqyAsync = async function() {
    if (audioTracks) audioTracks = undefined;
    var audioTracks = [];
    await importCmd5xAsync();
    var json = await doRequestAsync();
    var info = json.data.program.audio;
    info.forEach(function (item, index) {
        let _track = {};
        _track.bid = item.bid;
        _track.name = item.name;
        _track.cf = item.cf;
        _track.ct = item.ct;
        _track.lid = item.lid;
        audioTracks.push(_track);
    });
    audioTracks.sort((a1, a2) => {
        return (a2.name < a1.name ? 1 : (a2.name == a1.name ? 0 : -1)) + (a2.bid - a1.bid);
    });
    let p = "";
    audioTracks.forEach(function (item, index) {
        p += `\r\n[${index}]: {${item.name || ""}_${item.bid}_${item.cf}}`;
    });
    let _input = prompt("请选择音轨" + p);
    if (!_input) return;
    let _select = Number(_input);
    let _lid = audioTracks[_select].lid;
    let _ct = audioTracks[_select].ct;
    let _cf = audioTracks[_select].cf;
    var json = await doRequestAsync(_lid, _ct, _cf);
    var info = json.data.program.audio.find(a=>a._selected);
    var aSize = 0;
    var fs = info.fs;
    var content = "#EXTM3U\n";
    var prefix = "https://data.video.iqiyi.com/videos";
    let results = fs.map(async fs_i => {
        let url = fs_i.l;
        aSize += fs_i.b;
        let api = prefix + url;
        let t = "";
        if(json.data.boss_ts){
            t = json.data.boss_ts.data.t;
        }
        let vid = playerObject._player.package.engine.adproxy.engine.movieinfo.vid;
        let su = getCookie("QC005");
        api = prefix + url + "&t=" + t + "&vid=" + vid
                + "&QY00001=" + /qd_uid=(\d+)/g.exec(url)[1] + "&su="
                + su + "&ib=4&ibt=" + cmd5x(t + /\/(\w{10,})/g.exec(url)[1]) + "&ptime=0";
        let _j = await fetch(api).then(resp => resp.json());
        return "#EXTINF:"+(fs_i.d/1000).toFixed(2)+"\n" + _j["l"];
    });
    let urls = await Promise.all(results);
    content += urls.join('\n');
    content += "\n#EXT-X-ENDLIST";
    var title = "";
    try {
        title = document.querySelector('h1.player-title a.title-link[title]')['title'] + "_" + document.querySelector('h1.player-title em').innerText + `_${(info.name || "")}_${info.cf}_` + (aSize / 1024 / 1024).toFixed(2) + "MB.m3u8";
    } catch (error) {
        title = document.title + `_${(info.name || "")}_${info.cf}_` + (aSize / 1024 / 1024).toFixed(2) + "MB.m3u8";
    }
    download(content, title);
};
ndIqyAsync();
```
</details>


## 腾讯视频获取全集VID
<details><summary>点击查看 (上次更新: 2022/06/24)</summary>

```javascript
var myList = __pinia.episodeMain.listData[0].filter(e=>e.item_params.is_trailer=='0').map(e=>e.item_params.vid);
var a = prompt(`剧集VID列表(共${myList.length}项)`, myList.join(','));
```
</details>


## Wetv获取全集VID
<details><summary>点击查看 (上次更新: 2022/06/24)</summary>

```javascript
var myList = JSON.parse(__NEXT_DATA__.props.pageProps.data).videoList.filter(l=>l.default_pay_status!=null).map(e=>e.vid);
var a = prompt(`剧集VID列表(共${myList.length}项)`, myList.join(','));
```
</details>


## Wetv m3u8链接(普通不加密内容)
<details><summary>点击查看 (上次更新: 2022/06/24)</summary>

```javascript
var a = prompt(player.plugins.dataset.commonMap.videoName, player.currentVideoInfo.loadingUrl.replace(/:.*qq.com/g, "://defaultts.tc.qq.com/defaultts.tc.qq.com"));
```
</details>

## Wetv m3u8+所有语言字幕
<details><summary>点击查看 (上次更新: 2022/06/24)</summary>

```javascript
var download = function (text, filename, type) {
    let b = new Blob([text], {
      type: type || 'text/plain'
    });
    let a = document.createElement("a");
    a.href = URL.createObjectURL(b);
    a.setAttribute("download", filename);
    a.click();
};
var importJsZipAsync = async function () {
    let jsCode = await fetch("https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.0/jszip.min.js").then(resp => resp.text());
    let script = document.createElement("script");
    script.text = jsCode;
    document.getElementsByTagName("head")[0].appendChild(script);
};
var doWetv = async function () {
    /* 获取完整m3u8 */
    var vTitle = player.plugins.dataset.commonMap.videoName.replace("/","_").replace("\\","_");
    var m3u8Link = player.currentVideoInfo.loadingUrl.replace(/:.*qq.com/g, "://defaultts.tc.qq.com/defaultts.tc.qq.com");
    var m3u8Content = player.corePlayer._kernel._videoInfos[0]._extraVars.preloadVideoNode._hls.coreComponents.filter(c => c.details)[0].details.m3u8;
    for (let m of m3u8Content.match(/\n[^#]+/g)) {
        m3u8Content = m3u8Content.replace(m, "\n" + new URL(m.trim(), m3u8Link).href + "\n");
    }
    /* 处理drm */
    if(m3u8Content.indexOf("SAMPLE-AES-CTR") > -1) {
        m3u8Content =  WeTVPlayer.playReport.reporter.vInfo.vl.vi[0].ul.m3u8.replace(/https?:.*?qq.com/g, "https://defaultts.tc.qq.com/defaultts.tc.qq.com");
        var mName = vTitle + "[v+a].m3u8";
        var licUrl = WeTVPlayer.playReport.reporter.vInfo.vl.vi[0].ckc;
        m3u8Content = m3u8Content.replace("#EXTM3U","#EXTM3U\n"+"#EXT-WV-LIC-URL:" + licUrl);
    }
    var toastId = player.toast.call(null, {
        msg: "下载字幕中",
        timeout: 0,
        state: "loading"
    });
    /* 获取字幕信息 */
    var subtitles = [];
    if(WeTVPlayer.playReport.reporter.vInfo.sfl.cnt > 0) {
        var results = WeTVPlayer.playReport.reporter.vInfo.sfl.fi.map(async s => { return { name: s.name, ext: s.url.replace(/.m3u8.*/, "").split(".").pop(), content: await fetch(s.url.replace(/.m3u8.*/, "")).then(resp => resp.text()) } });
        subtitles = await Promise.all(results);
    }
    player.cleanToast(toastId);
    player.toast.call(null, {
        msg: "已完成解析, 准备下载",
        state: "success"
    });
    /* 引入JSZip */
    await importJsZipAsync();
    /* zip打包 */
    var zip = new JSZip();
    /* 塞入文件 */
    zip.file(`${vTitle}.m3u8`, m3u8Content);
    for (let s of subtitles) {
        zip.file(`${vTitle}.${s.name}.${s.ext}`, s.content);
    }
    zip.generateAsync({ type: "blob" }).then(function (content) {
        download(content, vTitle + ".zip", "application/octet-stream");
    });
};
doWetv();
```
</details>
