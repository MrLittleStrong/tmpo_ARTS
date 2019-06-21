近期在bug后台收到一条NSInternalInconsistencyException导致闪退的记录。

出错堆栈如下：-[AVCaptureStillImageOutput captureStillImageAsynchronouslyFromConnection:completionHandler:] Inconsistent state

同时注意到该错误全部发生在iPad上。

最后查到原因是在iPad上开启了画中画的情况下，使用AVCaptureSession拍照由于session处于被interrupt的状态，所以会导致闪退。还有其他的原因例如打电话，闹铃，处在分屏模式下等。

解决办法：在session startRunning前添加AVCaptureSessionWasInterrupted通知的观察。样例代码如下：

```objective-c

#pragma mark - Session Notifications

- (void) addObservers
{
    /*
     A session can only run when the app is full screen. It will be interrupted
     in a multi-app layout, introduced in iOS 9, see also the documentation of
     AVCaptureSessionInterruptionReason. Add observers to handle these session
     interruptions and show a preview is paused message. See the documentation
     of AVCaptureSessionWasInterruptedNotification for other interruption reasons.
     */
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(sessionWasInterrupted:) name:AVCaptureSessionWasInterruptedNotification object:self.session];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(sessionInterruptionEnded:) name:AVCaptureSessionInterruptionEndedNotification object:self.session];
}

- (void) removeObservers
{
    [[NSNotificationCenter defaultCenter] removeObserver:self];
    
}


- (void) sessionWasInterrupted:(NSNotification*)notification
{
    AVCaptureSessionInterruptionReason reason = [notification.userInfo[AVCaptureSessionInterruptionReasonKey] integerValue];
    NSLog(@"Capture session was interrupted with reason %ld", (long)reason);
    
    //在此禁止掉拍照功能，同时添加提醒
    if (reason == AVCaptureSessionInterruptionReasonAudioDeviceInUseByAnotherClient ||
        reason == AVCaptureSessionInterruptionReasonVideoDeviceInUseByAnotherClient) {
       
    }
    else if (reason == AVCaptureSessionInterruptionReasonVideoDeviceNotAvailableWithMultipleForegroundApps) {
       
        
    }
    else if (@available(iOS 11.1, *)) {
        if (reason == AVCaptureSessionInterruptionReasonVideoDeviceNotAvailableDueToSystemPressure) {
            
        }
    } else {
        // Fallback on earlier versions
    }
}

- (void) sessionInterruptionEnded:(NSNotification*)notification
{
   //在此可以恢复拍照功能
}
```

具体可以查看[苹果的样例代码](https://developer.apple.com/library/content/samplecode/AVCam/Listings/Objective_C_AVCam_AVCamCameraViewController_m.html#//apple_ref/doc/uid/DTS40010112-Objective_C_AVCam_AVCamCameraViewController_m-DontLinkElementID_7)