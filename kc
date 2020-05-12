!function () {
  'use strict';

  var dir;
  var codeInfo;
  var content;
  var file_id;
  var failed = 0;
  var html_btn = '<a class="g-button g-button-blue href="javascript:;" id="fast_link_btn" title="课程360转存" style="display: inline-block;"">';
  html_btn += '<span class="g-button-right">';
  html_btn += '<em class="icon icon-disk" title="课程360转存"></em>';
  html_btn += '<span class="text" style="width: auto;">课程360转存</span>'
  html_btn += '</span></a>';
  let loop = setInterval(() => {
    var html_tag = $("div.tcuLAu");
    if (!html_tag) return false;
    html_tag.append(html_btn);
    $("#fast_link_btn").click(function () {
      GetInfo();
    });
    clearInterval(loop);
  }, 2000);

  function DuParser() {}
  DuParser.parse = function generalDuCodeParse(szUrl) {
    var r;
    r = DuParser.parseDu_v3(szUrl);
    r.ver = 'PCS-Go';
    return r;
  };

  DuParser.parseDu_v3 = function parseDu_v3(szUrl) {
    return szUrl.split('****').map(function(z) {
      // unsigned long long: 0~18446744073709551615
      //console.log(z.fromBase64());
      return z.trim().fromBase64().match(/-length=([\d]{1,20}) -md5=([\da-f]{32}) -slicemd5=([\da-f]{32}) -crc32=([\d]{1,20}) "([\s\S]+)"/)
    }).filter(function(z) {
      return z;
    }).map(function(info) {
      return {
        md5: info[2],
        md5s: info[3],
        size: info[1],
        name: info[5]
      };
    });
  };



  function GetInfo(str='') {
    Swal.fire({
      title: '请输入提取码',
      input: 'textarea',
      inputValue: str,
      showCancelButton: true,
      inputPlaceholder: '[支持批量]',
      confirmButtonText: '确定',
      cancelButtonText: '取消',
      inputValidator: (value) => {
        if (!value) {
          return '链接不能为空';
        }
        codeInfo = DuParser.parse(value);
        if(!codeInfo.length){
          return '未识别到正确的链接';
        }
      }
    }).then((result1)=>{
      if (result1.dismiss){
        return;
      }
      Process();
    });
  }

  function Process(){
    dir = GM_getValue('last_dir');
    if(!dir){dir = '';}
    Swal.fire({
      title: '请输入保存路径',
      input: 'text',
      inputPlaceholder: '格式示例：/GTA5/，默认保存在根目录',
      inputValue: dir,
      showCancelButton: true,
      confirmButtonText: '确定',
      cancelButtonText: '取消',
    }).then((result2)=>{
      if (result2.dismiss){
        return;
      }
      if(result2){
        dir = result2.value;
        GM_setValue('last_dir', dir);
        if (dir.charAt(dir.length - 1)!='/'){
          dir=dir+'/';
        }
      }
      Swal.fire({
        title: '文件提取中',
        html: '正在转存第 <file_id></file_id> 个',
        onBeforeOpen: () => {
          Swal.showLoading()
          content = Swal.getContent();
          if (content){
            file_id = content.querySelector('file_id');
            saveFile(0, false);
          }
        }
      });
    });
  }

  function GetInfo_url(){
    var bdlink = href.match(/[\?#]bdlink=([\da-zA-Z/\+]+)&?/);
    if(bdlink){
      bdlink = bdlink[1].fromBase64();
      GetInfo(bdlink);
    }
  }

  const href = window.location.href;

  document.addEventListener('DOMContentLoaded', GetInfo_url);
}();
