# 密碼小眼睛的功能(angular)with 記住帳號(cookie)

###### tags: `工作筆記`、 `angular`

[Demo完整畫面](https://stackblitz.com/edit/angular-ivy-ydakja?file=src%2Fapp%2Fapp.component.html)

密碼小眼睛的功能

``` html
 <div class="input-group">
    <div class="div">
      <input name="pw" [(ngModel)]="password" required type="password" id="pw" class="form-control">
      <label for="" placeholder="密碼:" alt="密碼"></label>
    </div>
    <button type="button" id="btnToggle" class="toggle" (click)="togglePassword()">
      <i id="eyeIcon" class="fa fa-eye"></i>
    </button>
</div>
```

``` javascript
togglePassword() {
    let eyeTag = this.el.nativeElement.querySelector(".fa"); 
    let pw = this.el.nativeElement.querySelector("#pw"); 

    if(!eyeTag.classList.contains('fa-eye'))
    {
      eyeTag.classList.remove('fa-eye-slash'); 
      eyeTag.classList.add('fa-eye'); 
      pw.setAttribute('type', 'password');
    }
    else
    {
      eyeTag.classList.add('fa-eye-slash'); 
      eyeTag.classList.remove('fa-eye'); 
      pw.setAttribute('type', 'text');
    }
  }
```

記住帳號功能
```htmlembedded=
<a id="btn_login" class="loginbtn" href="javascript:void(0)" (click)="Login()">登入</a>

```

``` javascript

// 初始化取回已記憶的帳號
ngOnInit() {
    this.SetloginCookieValue();
  }
  
  SetloginCookieValue(){
    const tmp = this.GetCookie('useremail');
    const check = this.GetCookie('remembercheck');
    if(tmp !== null) {
      this.email = tmp;
    }
    this.remember = check === 'true' ? true: false;
  }

  GetCookie(canme) {
    const arg = canme + '=';
    const alen = arg.length;
    const clen = document.cookie.length;
    let i = 0;
    while(i < clen) {
      const j = i + alen;
      if(document.cookie.substring(i, j) === arg) return this.getCookieVal(j);
      i = document.cookie.indexOf(' ', i) + 1;
      if(i === 0) break;
    }
    return null;
  }

  getCookieVal(offset) {
    let endstr = document.cookie.indexOf(';', offset);
    if(endstr === -1) endstr = document.cookie.length;
    return unescape(document.cookie.substring(offset, endstr));
  }
  
  
  Login() {
    this.SetAccountCookie();
  }
  
 SetAccountCookie() {
    const now = new Date();
    now.setMonth( now.getMonth() + 1 );
    now.setTime(now.getTime()-1);	// 設定 Cookie 的失效時間比目前時間還早

    if(this.remember){
      document.cookie = 'useremail=' + this.email + '; Expires=' + now.toUTCString ()+ ';path=/';
      document.cookie = 'remembercheck=' + this.remember + '; Expires=' + now.toUTCString ()+ ';path=/';
    }

    if(!this.remember){
      document.cookie = 'useremail=' + ';path=/';
      document.cookie = 'remembercheck=' + this.remember + ';path=/';
    }
  }

```