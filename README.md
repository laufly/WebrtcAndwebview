关于webrtc js端sdk在安卓webview的测试:项目是相关代码:



chrome的WebRTC开发文档，发现只有36版本的WebView才支持WebRTC。通过安卓5.0成功在系统内置浏览器中调用了摄像头与麦克风

通过写入安卓权限，成功在5.0的系统中通过调用系统WebRTC内核实现视频语音聊天。但对系统版本要求较高，兼容性差

结论:chromium 内核从  M36开始支持webrtc。  Android 4.4 开始webview使用 chromium内核。    Android 5.0也就是API 21中的webview使用的是chromium M37版本。

参考文档:

       https://www.cnblogs.com/linl/p/4235001.html

       http://blog.csdn.net/typename/article/details/40425275

       http://blog.csdn.net/ren65432/article/details/53815832


   注意:
   1/ 如果想兼容4.0到5.0需要提取安卓5.0的WebView内核，并成功移植到项目中直接使用移植来的WebView，成功在安卓4.0-5.0所有平台上成功完成视频、语音聊天

           2/ onPermissionRequest 回调中不能调用super的方法.


   参考代码:

  1.
```
  webView.setWebChromeClient(new WebChromeClient() {
             @Override
             public void onPermissionRequest(final PermissionRequest request) {

                 MainActivity.this.runOnUiThread(new Runnable(){
                     @TargetApi(Build.VERSION_CODES.LOLLIPOP)
                     @Override
                     public void run() {
                         request.grant(request.getResources());
                     }// run
                 });// MainActivity

             }// onPermissionRequest

         });

```
  2. 权限
  ```
         <uses-permission android:name="android.permission.INTERNET"/>
            <uses-permission android:name="android.permission.CAMERA" />
            <uses-permission android:name="android.permission.CAPTURE_VIDEO_OUTPUT" />
  ```