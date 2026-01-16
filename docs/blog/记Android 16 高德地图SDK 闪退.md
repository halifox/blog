
## 依赖版本
```
    implementation("com.amap.api:3dmap:9.8.3")
```

## 错误堆栈
```
Redmi/dali/dali:16/BP2A.250605.031.A3/OS3.0.7.0.WONCNXM:user/release-keys
Abort message: 'Pointer tag for <addr> was truncated, see 'https://source.android.com/devices/tech/debug/tagged-pointers'.'

      #00 pc 0000000000072d3c  /apex/com.android.runtime/lib64/bionic/libc.so (abort+156) (BuildId: ee2a16e13d7f3eb9da4980a5d1c3f92b)
      #01 pc 00000000000595a8  /apex/com.android.runtime/lib64/bionic/libc.so (free+104) (BuildId: ee2a16e13d7f3eb9da4980a5d1c3f92b)
      #02 pc 00000000007c0690  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk!libAMapSDK_MAP_v9_8_3.so (offset 0x578000) (BuildId: 39f079b42910c63b85d73f04e316a5cc792e3970)
      #03 pc 00000000007bfff4  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk!libAMapSDK_MAP_v9_8_3.so (offset 0x578000) (BuildId: 39f079b42910c63b85d73f04e316a5cc792e3970)
      #04 pc 00000000007ce7f4  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk!libAMapSDK_MAP_v9_8_3.so (offset 0x578000) (BuildId: 39f079b42910c63b85d73f04e316a5cc792e3970)
      #05 pc 000000000040b8a4  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk!libAMapSDK_MAP_v9_8_3.so (offset 0x578000) (BuildId: 39f079b42910c63b85d73f04e316a5cc792e3970)
      #06 pc 000000000038ca4c  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk!libAMapSDK_MAP_v9_8_3.so (offset 0x578000) (BuildId: 39f079b42910c63b85d73f04e316a5cc792e3970)
      #07 pc 00000000003b0c14  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk!libAMapSDK_MAP_v9_8_3.so (offset 0x578000) (BuildId: 39f079b42910c63b85d73f04e316a5cc792e3970)
      #08 pc 00000000003b06dc  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk!libAMapSDK_MAP_v9_8_3.so (offset 0x578000) (BuildId: 39f079b42910c63b85d73f04e316a5cc792e3970)
      #09 pc 00000000003b0550  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk!libAMapSDK_MAP_v9_8_3.so (offset 0x578000) (BuildId: 39f079b42910c63b85d73f04e316a5cc792e3970)
      #10 pc 0000000000371d0c  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk!libAMapSDK_MAP_v9_8_3.so (offset 0x578000) (BuildId: 39f079b42910c63b85d73f04e316a5cc792e3970)
      #11 pc 00000000002a187c  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk!libAMapSDK_MAP_v9_8_3.so (offset 0x578000) (BuildId: 39f079b42910c63b85d73f04e316a5cc792e3970)
      #12 pc 0000000000212b2c  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk!libAMapSDK_MAP_v9_8_3.so (offset 0x578000) (BuildId: 39f079b42910c63b85d73f04e316a5cc792e3970)
      #13 pc 0000000000212dc4  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk!libAMapSDK_MAP_v9_8_3.so (offset 0x578000) (BuildId: 39f079b42910c63b85d73f04e316a5cc792e3970)
      #14 pc 00000000001e74e8  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk!libAMapSDK_MAP_v9_8_3.so (offset 0x578000) (BuildId: 39f079b42910c63b85d73f04e316a5cc792e3970)
      #15 pc 000000000018d8a4  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk!libAMapSDK_MAP_v9_8_3.so (offset 0x578000) (BuildId: 39f079b42910c63b85d73f04e316a5cc792e3970)
      #16 pc 000000000011e724  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk!libAMapSDK_MAP_v9_8_3.so (offset 0x578000) (Java_com_autonavi_base_ae_gmap_GLMapEngine_nativeDestroy+72) (BuildId: 39f079b42910c63b85d73f04e316a5cc792e3970)
      #17 pc 0000000000363e50  /system/framework/arm64/boot.oat (art_jni_trampoline+112) (BuildId: c7e7e6faa51bb8d9305dad9be682fdc7baeb097b)
      #18 pc 00000000006a8d8c  /apex/com.android.art/lib64/libart.so (nterp_helper+1948) (BuildId: b369ace762dbc30337aa79f5b81a8c15)
      #19 pc 00000000001fe800  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk (offset 0x1000) (com.autonavi.base.ae.gmap.GLMapEngine.destroyAMapEngine+100)
      #20 pc 00000000006a9544  /apex/com.android.art/lib64/libart.so (nterp_helper+3924) (BuildId: b369ace762dbc30337aa79f5b81a8c15)
      #21 pc 00000000001b909c  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk (offset 0x1000) (com.amap.api.col.3l.cb.destroySurface+104)
      #22 pc 00000000006a9544  /apex/com.android.art/lib64/libart.so (nterp_helper+3924) (BuildId: b369ace762dbc30337aa79f5b81a8c15)
      #23 pc 00000000001b31ee  /data/app/~~kYkzfXCe00oHm0xfIput5Q==/com.dingli.thingsboard--JRuS9x6B2tqtpLsslb67Q==/base.apk (offset 0x1000) (com.amap.api.col.3l.va.run+46)
      #24 pc 00000000006aa364  /apex/com.android.art/lib64/libart.so (nterp_helper+7540) (BuildId: b369ace762dbc30337aa79f5b81a8c15)
      #25 pc 0000000000238760  /system/framework/framework.jar (offset 0x124c000) (android.opengl.GLSurfaceView$GLThread.guardedRun+1460)
      #26 pc 00000000006a9544  /apex/com.android.art/lib64/libart.so (nterp_helper+3924) (BuildId: b369ace762dbc30337aa79f5b81a8c15)
      #27 pc 00000000002394fe  /system/framework/framework.jar (offset 0x124c000) (android.opengl.GLSurfaceView$GLThread.run+122)
      #28 pc 0000000000363f94  /apex/com.android.art/lib64/libart.so (art_quick_invoke_stub+612) (BuildId: b369ace762dbc30337aa79f5b81a8c15)
      #29 pc 000000000028fab8  /apex/com.android.art/lib64/libart.so (art::ArtMethod::Invoke(art::Thread*, unsigned int*, unsigned int, art::JValue*, char const*)+216) (BuildId: b369ace762dbc30337aa79f5b81a8c15)
      #30 pc 000000000048c6f4  /apex/com.android.art/lib64/libart.so (art::Thread::CreateCallback(void*)+1364) (BuildId: b369ace762dbc30337aa79f5b81a8c15)
      #31 pc 00000000000840fc  /apex/com.android.runtime/lib64/bionic/libc.so (__pthread_start(void*)+236) (BuildId: ee2a16e13d7f3eb9da4980a5d1c3f92b)
      #32 pc 0000000000076620  /apex/com.android.runtime/lib64/bionic/libc.so (__start_thread+64) (BuildId: ee2a16e13d7f3eb9da4980a5d1c3f92b)
```

### 1. 核心原因分析

**崩溃信息：**
`Abort message: 'Pointer tag for <addr> was truncated...'`

**技术背景：**
从 Android 11 开始，Google 在 64 位 ARM 设备上默认启用了 **Top-byte Ignore (TBI)** 和 **Tagged Pointers** 技术。

* **机制：** 系统会在指针的最高 8 位（Top byte）存储一些元数据（Tag），用于内存调试和安全检查。
* **错误原因：** 当代码尝试 `free()` 或操作一个指针时，如果该指针的最高位被应用程序错误地修改、清除或截断（Truncated），Bionic 库（libc.so）会检测到 Tag 不匹配。为了防止潜在的堆溢出或内存损坏攻击，系统会直接触发 `abort` 崩溃。

**堆栈路径：**
从堆栈 `#02` 到 `#16` 可以看到，崩溃发生在 **高德地图 SDK (`libAMapSDK_MAP_v9_8_3.so`)** 内部。具体是在执行 `nativeDestroy`（销毁地图引擎）并调用 `free` 释放内存时触发的。


### 解决方案：在 Manifest 中关闭内存标记检查

```
<application
    android:allowNativeHeapPointerTagging="false"
    ...>
</application>
```