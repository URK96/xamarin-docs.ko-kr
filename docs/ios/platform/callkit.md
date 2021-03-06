---
title: CallKit
description: 이 문서에서는 해당 Apple iOS 10과 Xamarin.iOS VOIP 응용 프로그램에서 구현 하는 방법에서 릴리스는 새로운 CallKit API를 설명 합니다.
ms.prod: xamarin
ms.assetid: 738A142D-FFD2-4738-B3ED-57C273179848
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 67c761aa6656b571f16632dd1a076ff11737a424
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="callkit"></a>CallKit

_이 문서에서는 해당 Apple iOS 10과 Xamarin.iOS VOIP 응용 프로그램에서 구현 하는 방법에서 릴리스는 새로운 CallKit API를 설명 합니다._


IOS 10에서에서 새 CallKit API VOIP 앱 iPhone UI와 통합 하 고 친숙 한 인터페이스를 제공 하 고 최종 사용자에 게 발생 하는 방법을 제공 합니다. 이 api 사용자 수 보기 및 iOS 장치 잠금 화면에서 VOIP 전화와 상호 작용 및 관리 전화 앱을 사용 하 여 연락처 **즐겨찾기** 및 **제외할지** 뷰.

## <a name="about-callkit"></a>CallKit에 대 한

Apple에 따라 CallKit 자사 iOS 10에서 제공 되는 파티에 타사 음성을 통해 IP VOIP () 앱 상승 되는 새로운 프레임 워크는 합니다. CallKit API VOIP 앱을을 iPhone UI와 통합 하 고 친숙 한 인터페이스를 제공 하 고 최종 사용자에 게 발생할 수 있습니다. 기본 제공 전화 앱에서와 마찬가지로 사용자 보기 및 iOS 장치 잠금 화면에서 VOIP 전화와 상호 작용 하 고 관리할 수 전화 앱을 사용 하 여 연락처 **즐겨찾기** 및 **제외할지** 뷰.

또한 CallKit API 이름 (호출자 ID)와 전화 번호를 연결 하거나 숫자 해야 하는 경우 시스템 차단 (차단 호출)을 알 수 있는 앱 확장을 만드는 기능을 제공 합니다.

### <a name="the-existing-voip-app-experience"></a>기존 VOIP 앱 경험

새 CallKit API 및 해당 기능을 설명 하기 전에 세 번째 서비스로 현재 사용자 환경에 iOS에서 앱 VOIP에 9 (및 더 작은 값) MonkeyCall 라는 가상의 VOIP 앱을 사용 하 여 파티 합니다. MonkeyCall 기존 iOS Api를 사용 하는 VOIP 전화를 주고받을 수 있도록 하는 간단한 앱입니다.

현재 사용자 MonkeyCall에서 들어오는 호출을 수신 하는 경우 해당 iPhone 잠겨 잠금 화면에서 받은 알림 구분 되지 않습니다 다른 종류의 알림 (메시지에서 처럼 앱을 예를 들어 메일 또는).

호출 응답 하려는 앱을 열고 호출을 수락 하 고 대화를 시작할 수 전에 휴대폰의 잠금을 해제 하려면 해당 암호 (또는 사용자 Touch ID)를 입력 하려면 MonkeyCall 알림을 않도록 있어야 합니다.

경험 하는 것이 전화를 잠금 해제 된 경우에 동일 하 게 복잡 합니다. 마찬가지로 수신 MonkeyCall 전화 화면 위쪽에서의 슬라이드 표준 알림 배너를으로 표시 됩니다. 알림을 임시 이므로,이 누락 될 수 있습니다 쉽게 알림 센터를 열고 또는 응답 한 다음 호출 또는 찾기 및 MonkeyCall 응용 프로그램을 수동으로 실행을 특정 알림의 찾을 해야만 사용자가 합니다.

### <a name="the-callkit-voip-app-experience"></a>CallKit VOIP 앱 경험

MonkeyCall 응용 프로그램에서 새로운 CallKit Api를 구현 하 여 들어오는 VOIP 호출으로는 사용자의 경험 iOS 10에서에서 크게 높일 수 있습니다. 사용자 전화 번호 위에서 잠겨 있을 때 VOIP 전화를 받지를 예로 들어를 보겠습니다. CallKit를 구현 하 여 호출 된 전체 화면, 네이티브 UI 및 표준 살짝 대답 기능이 기본 제공 전화 응용 프로그램에서 수신 된와 마찬가지로 호출 iPhone의 잠금 화면에 표시 됩니다.

다시 MonkeyCall VOIP 전화를 받을 때 iPhone 잠금이 해제 되어 있으면 동일한 전체 화면, 네이티브 UI 및 기본 제공 앱 제공 되는 전화 및 MonkeyCall의 표준 살짝 대답 및 tap-거부 기능에 있는 옵션이 사용자 지정 벨 소리를 재생 .

다른 유형의 호출에 기본 제공 제외할지 상호 작용에 대 한 추가 기능을 해당 VOIP 허용 MonkeyCall 호출이 항목이 나 즐겨찾기 목록 CallKit 제공, 기본 제공 방해 하지 및 차단 기능을 사용 하려면 Siri에서 MonkeyCall 호출을 시작 하 고 연락처 앱에서 사용자에 대 한 MonkeyCall 호출을 할당 하는 사용자에 대 한 기능을 제공 합니다.

다음 섹션에서는 CallKit 아키텍처를 설명 하는, 들어오고 나가는 자세히 흐름과 CallKit API 호출 합니다.


## <a name="the-callkit-architecture"></a>CallKit 아키텍처

10 ios에서는 Apple는 채택 했습니다 CallKit을 시스템 서비스의 모든를 예를 들어 CarPlay에 대 한 호출을 통해 CallKit 시스템 UI로 알려져 있습니다. MonkeyCall CallKit를 채택 하므로, 아래 제공 된 예제에서는 이러한 기본 제공 시스템 서비스와 동일한 방법으로 시스템에 인식 하 고 동일한 기능을 모두 가져옵니다.

[![](callkit-images/callkit01.png "CallKit 서비스 스택")](callkit-images/callkit01.png#lightbox)

위의 다이어그램에서 MonkeyCall 응용 프로그램에 자세히 살펴보겠습니다. 응용 프로그램 자체 네트워크와 통신 하는 코드를 모두 포함 되 고 자체 사용자 인터페이스를 포함 합니다. 시스템과 통신 하는 CallKit에 연결 합니다.

[![](callkit-images/callkit02.png "MonkeyCall 응용 프로그램 아키텍처")](callkit-images/callkit02.png#lightbox)

CallKit 앱을 사용 하는 두 가지 주요 인터페이스 되어 있습니다.

- `CXProvider` -이 발생할 수 있는 모든 대역폭을 벗어난 알림 시스템 알리기 위해 MonkeyCall 앱을 수 있습니다.
- `CXCallController` -시스템 로컬 사용자 작업에 알리기 위해 MonkeyCall 앱을 있습니다.

### <a name="the-cxprovider"></a>CXProvider

위의 설명 대로 `CXProvider` 시스템 발생할 수 있는 모든 대역폭을 벗어난 알림이에 알리기 위해 앱 할 수 있습니다. 로컬 사용자 작업으로 인해 발생 하지 않는 되지만 외부 이벤트 들어오는 호출 등으로 인해 발생 하는 알림입니다.

앱을 사용할지는 `CXProvider` 다음에 대 한 합니다.

- 시스템에 대 한 들어오는 호출을 보고 합니다.
- 보고 하는 호출을 나가는 시스템에 연결 합니다.
- 시스템에 대 한 호출을 종료 하는 원격 사용자를 보고 합니다.

사용 하 여 응용 프로그램을 시스템에 전달 하려는 경우는 `CXCallUpdate` 클래스 및 사용 하 여 시스템을 앱과 통신 해야 하는 경우는 `CXAction` 클래스:

[![](callkit-images/callkit03.png "CXProvider 통해 시스템과 통신합니다.")](callkit-images/callkit03.png#lightbox)

### <a name="the-cxcallcontroller"></a>CXCallController

`CXCallController` 시스템 VOIP 통화 시작 사용자 등의 로컬 사용자 작업에 알리기 위해 앱 할 수 있습니다. 구현 하 여 한 `CXCallController` 시스템에서 다른 유형의 호출 상호 작용 하는 앱을 가져옵니다. 예를 들어는 이미 진행 중인 활성 전화 통신 호출 `CXCallController` VOIP 응용 프로그램을 호출 하는 대기 중으로 배치를 시작 하거나 VOIP 호출에 응답을 허용할 수 있습니다.

앱을 사용할지는 `CXCallController` 다음에 대 한 합니다.

- 사용자가 시스템에 대 한 보내는 호출을 시작할 때 보고서입니다.
- 사용자 시스템으로 들어오는 호출에 응답 하는 경우 보고서입니다.
- 사용자 시스템에 대 한 호출을 종료 될 때 보고서입니다.

사용 하 여 응용 프로그램에서 로컬 사용자 작업은 시스템을 통신 하려는 `CXTransaction` 클래스:

[![](callkit-images/callkit04.png "CXCallController를 사용 하 여 시스템에 보고")](callkit-images/callkit04.png#lightbox)

## <a name="implementing-callkit"></a>CallKit 구현

다음 섹션에서는 CallKit Xamarin.iOS VOIP 응용 프로그램에서 구현 하는 방법을 표시 됩니다. 예에서이 문서는 가상의 MonkeyCall VOIP 앱에서 코드를 사용 합니다. 여기에 제공 된 코드 여러 지원 클래스를 나타냅니다. 특정 부분 됩니다 CallKit 다음 섹션에서 자세히 다룹니다.

### <a name="the-activecall-class"></a>ActiveCall 클래스

`ActiveCall` 클래스는 다음과 같이 현재 활성화 된 VOIP 호출에 대 한 정보를 모두 보유할 MonkeyCall 응용 프로그램에서 사용 됩니다.

```csharp
using System;
using CoreFoundation;
using Foundation;

namespace MonkeyCall
{
    public class ActiveCall
    {
        #region Private Variables
        private bool isConnecting;
        private bool isConnected;
        private bool isOnhold;
        #endregion

        #region Computed Properties
        public NSUuid UUID { get; set; }
        public bool isOutgoing { get; set; }
        public string Handle { get; set; }
        public DateTime StartedConnectingOn { get; set;}
        public DateTime ConnectedOn { get; set;}
        public DateTime EndedOn { get; set; }

        public bool IsConnecting {
            get { return isConnecting; }
            set {
                isConnecting = value;
                if (isConnecting) StartedConnectingOn = DateTime.Now;
                RaiseStartingConnectionChanged ();
            }
        }

        public bool IsConnected {
            get { return isConnected; }
            set {
                isConnected = value;
                if (isConnected) {
                    ConnectedOn = DateTime.Now;
                } else {
                    EndedOn = DateTime.Now;
                }
                RaiseConnectedChanged ();
            }
        }

        public bool IsOnHold {
            get { return isOnhold; }
            set {
                isOnhold = value;
            }
        }
        #endregion

        #region Constructors
        public ActiveCall ()
        {
        }

        public ActiveCall (NSUuid uuid, string handle, bool outgoing)
        {
            // Initialize
            this.UUID = uuid;
            this.Handle = handle;
            this.isOutgoing = outgoing;
        }
        #endregion

        #region Public Methods
        public void StartCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call starting successfully
            completionHandler (true);

            // Simulate making a starting and completing a connection
            DispatchQueue.MainQueue.DispatchAfter (new DispatchTime(DispatchTime.Now, 3000), () => {
                // Note that the call is starting
                IsConnecting = true;

                // Simulate pause before connecting
                DispatchQueue.MainQueue.DispatchAfter (new DispatchTime (DispatchTime.Now, 1500), () => {
                    // Note that the call has connected
                    IsConnecting = false;
                    IsConnected = true;
                });
            });
        }

        public void AnswerCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call being answered
            IsConnected = true;
            completionHandler (true);
        }

        public void EndCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call ending
            IsConnected = false;
            completionHandler (true);
        }
        #endregion

        #region Events
        public delegate void ActiveCallbackDelegate (bool successful);
        public delegate void ActiveCallStateChangedDelegate (ActiveCall call);

        public event ActiveCallStateChangedDelegate StartingConnectionChanged;
        internal void RaiseStartingConnectionChanged ()
        {
            if (this.StartingConnectionChanged != null) this.StartingConnectionChanged (this);
        }

        public event ActiveCallStateChangedDelegate ConnectedChanged;
        internal void RaiseConnectedChanged ()
        {
            if (this.ConnectedChanged != null) this.ConnectedChanged (this);
        }
        #endregion
    }
}
```

`ActiveCall` 호출 상태가 변경 될 때 발생할 수 있는 두 개의 이벤트 및 호출의 상태를 정의 하는 몇 가지 속성을 보유 합니다. 예제 일 뿐 이므로, 시작에 응답 하 고 통화 종료를 시뮬레이션 하는 데 사용 되는 세 가지 메서드가 있습니다.

### <a name="the-startcallrequest-class"></a>StartCallRequest 클래스

`StartCallRequest` 정적 클래스는 나가는 작업할 호출할 때 사용 될 몇 가지 도우미 메서드를 제공 합니다.

```csharp
using System;
using Foundation;
using Intents;

namespace MonkeyCall
{
    public static class StartCallRequest
    {
        public static string URLScheme {
            get { return "monkeycall"; }
        }

        public static string ActivityType {
            get { return INIntentIdentifier.StartAudioCall.GetConstant ().ToString (); }
        }

        public static string CallHandleFromURL (NSUrl url)
        {
            // Is this a MonkeyCall handle?
            if (url.Scheme == URLScheme) {
                // Yes, return host
                return url.Host;
            } else {
                // Not handled
                return null;
            }
        }

        public static string CallHandleFromActivity (NSUserActivity activity)
        {
            // Is this a start call activity?
            if (activity.ActivityType == ActivityType) {
                // Yes, trap any errors
                try {
                    // Get first contact
                    var interaction = activity.GetInteraction ();
                    var startAudioCallIntent = interaction.Intent as INStartAudioCallIntent;
                    var contact = startAudioCallIntent.Contacts [0];

                    // Get the person handle
                    return contact.PersonHandle.Value;
                } catch {
                    // Error, report null
                    return null;
                }
            } else {
                // Not handled
                return null;
            }
        }
    }
}
```

`CallHandleFromURL` 및 `CallHandleFromActivity` 나가는 호출에서 호출 되는 사용자의 연락처 핸들을 가져올 수는 AppDelegate 클래스를 사용 합니다. 자세한 내용은 참조 하십시오는 [보내는 호출을 처리](#Handling-Outgoing-Calls) 아래 섹션.

### <a name="the-activecallmanager-class"></a>ActiveCallManager 클래스

`ActiveCallManager` MonkeyCall 응용 프로그램에서 열려 있는 모든 호출을 처리 하는 클래스입니다.

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using CallKit;

namespace MonkeyCall
{
    public class ActiveCallManager
    {
        #region Private Variables
        private CXCallController CallController = new CXCallController ();
        #endregion

        #region Computed Properties
        public List<ActiveCall> Calls { get; set; }
        #endregion

        #region Constructors
        public ActiveCallManager ()
        {
            // Initialize
            this.Calls = new List<ActiveCall> ();
        }
        #endregion

        #region Private Methods
        private void SendTransactionRequest (CXTransaction transaction)
        {
            // Send request to call controller
            CallController.RequestTransaction (transaction, (error) => {
                // Was there an error?
                if (error == null) {
                    // No, report success
                    Console.WriteLine ("Transaction request sent successfully.");
                } else {
                    // Yes, report error
                    Console.WriteLine ("Error requesting transaction: {0}", error);
                }
            });
        }
        #endregion

        #region Public Methods
        public ActiveCall FindCall (NSUuid uuid)
        {
            // Scan for requested call
            foreach (ActiveCall call in Calls) {
                if (call.UUID == uuid) return call;
            }

            // Not found
            return null;
        }

        public void StartCall (string contact)
        {
            // Build call action
            var handle = new CXHandle (CXHandleType.Generic, contact);
            var startCallAction = new CXStartCallAction (new NSUuid (), handle);

            // Create transaction
            var transaction = new CXTransaction (startCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void EndCall (ActiveCall call)
        {
            // Build action
            var endCallAction = new CXEndCallAction (call.UUID);

            // Create transaction
            var transaction = new CXTransaction (endCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void PlaceCallOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, true);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void RemoveCallFromOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, false);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }
        #endregion
    }
}
```

다시만 시뮬레이션 이므로는 `ActiveCallManager` 만의 컬렉션을 유지 `ActiveCall` 개체에 의해 지정된 된 호출을 찾기 위한 루틴이 및 해당 `UUID` 속성입니다. 시작, 종료 및 나가는 호출의-대기 상태를 변경 하는 메서드도 포함 됩니다. 자세한 내용은 참조 하십시오는 [보내는 호출을 처리](#Handling-Outgoing-Calls) 아래 섹션.

### <a name="the-providerdelegate-class"></a>ProviderDelegate 클래스

위에서 설명 했 듯이 `CXProvider` 대역의 알림에 대 한 응용 프로그램 및 시스템 간의 양방향 통신을 제공 합니다. 개발자가 사용자 지정 제공 `CXProviderDelegate` 에 연결 하 고는 `CXProvider` 밴드의 범위를 벗어난 CallKit 이벤트를 처리 하는 앱에 대 한 합니다. 다음을 사용 하 여 MonkeyCall `CXProviderDelegate`:

```csharp
using System;
using Foundation;
using CallKit;
using UIKit;

namespace MonkeyCall
{
    public class ProviderDelegate : CXProviderDelegate
    {
        #region Computed Properties
        public ActiveCallManager CallManager { get; set;}
        public CXProviderConfiguration Configuration { get; set; }
        public CXProvider Provider { get; set; }
        #endregion

        #region Constructors
        public ProviderDelegate (ActiveCallManager callManager)
        {
            // Save connection to call manager
            CallManager = callManager;

            // Define handle types
            var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };

            // Get Image Mask
            var maskImage = UIImage.FromFile ("telephone_receiver.png");

            // Setup the initial configurations
            Configuration = new CXProviderConfiguration ("MonkeyCall") {
                MaximumCallsPerCallGroup = 1,
                SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
                IconMaskImageData = maskImage.AsPNG(),
                RingtoneSound = "musicloop01.wav"
            };

            // Create a new provider
            Provider = new CXProvider (Configuration);

            // Attach this delegate
            Provider.SetDelegate (this, null);

        }
        #endregion

        #region Override Methods
        public override void DidReset (CXProvider provider)
        {
            // Remove all calls
            CallManager.Calls.Clear ();
        }

        public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
        {
            // Create new call record
            var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

            // Monitor state changes
            activeCall.StartingConnectionChanged += (call) => {
                if (call.isConnecting) {
                    // Inform system that the call is starting
                    Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNsDate());
                }
            };

            activeCall.ConnectedChanged += (call) => {
                if (call.isConnected) {
                    // Inform system that the call has connected
                    provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNsDate ());
                }
            };

            // Start call
            activeCall.StartCall ((successful) => {
                // Was the call able to be started?
                if (successful) {
                    // Yes, inform the system
                    action.Fulfill ();

                    // Add call to manager
                    CallManager.Calls.Add (activeCall);
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.AnswerCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.EndCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Remove call from manager's queue
                    CallManager.Calls.Remove (call);

                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformSetHeldCallAction (CXProvider provider, CXSetHeldCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Update hold status
            call.isOnHold = action.OnHold;

            // Inform system of success
            action.Fulfill ();
        }

        public override void TimedOutPerformingAction (CXProvider provider, CXAction action)
        {
            // Inform user that the action has timed out
        }

        public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // Start the calls audio session here
        }

        public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // End the calls audio session and restart any non-call
            // related audio
        }
        #endregion

        #region Public Methods
        public void ReportIncomingCall (NSUuid uuid, string handle)
        {
            // Create update to describe the incoming call and caller
            var update = new CXCallUpdate ();
            update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

            // Report incoming call to system
            Provider.ReportNewIncomingCall (uuid, update, (error) => {
                // Was the call accepted
                if (error == null) {
                    // Yes, report to call manager
                    CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
                } else {
                    // Report error to user here
                    Console.WriteLine ("Error: {0}", error);
                }
            });
        }
        #endregion
    }
}
```

이 대리자의 인스턴스를 만들 때 전달 되는 `ActiveCallManager` 모든 통화 활동 처리를 사용 합니다. 핸들 형식은 다음으로 정의 합니다 (`CXHandleType`) 하는 `CXProvider` 에 응답 합니다.

```csharp
// Define handle types
var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };
```

및는 호출이 진행 중인 경우 응용 프로그램의 아이콘에 적용 될 마스크를 가져옵니다.

```csharp
// Get Image Mask
var maskImage = UIImage.FromFile ("telephone_receiver.png");
```

이러한 값에 제공 가져오기는 `CXProviderConfiguration` 구성에 사용할는 `CXProvider`:

```csharp
// Setup the initial configurations
Configuration = new CXProviderConfiguration ("MonkeyCall") {
    MaximumCallsPerCallGroup = 1,
    SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
    IconMaskImageData = maskImage.AsPNG(),
    RingtoneSound = "musicloop01.wav"
};
```

대리자 다음 원본 또는 `CXProvider` 이러한 구성으로 자신을 연결 합니다.

```csharp
// Create a new provider
Provider = new CXProvider (Configuration);

// Attach this delegate
Provider.SetDelegate (this, null);
```

CallKit을 사용할 경우 응용 프로그램은 더 이상 만들고 오디오 자체 세션 처리을 대신 구성 하 고는 오디오 세션 시스템 만들고 것에 대 한 핸들을 사용 해야 합니다. 

실제 앱 인 경우는 `DidActivateAudioSession` 메서드는 미리 구성 된로 호출을 시작 하는 데 사용할 `AVAudioSession` 시스템 제공:

```csharp
public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // Start the call's audio session here...
}
```

사용할 것는 `DidDeactivateAudioSession` 메서드 마무리 하 고 시스템에 연결 해제를 오디오 세션을 제공 합니다.

```csharp
public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // End the calls audio session and restart any non-call
    // releated audio
}
```

코드의 나머지 부분에 나오는 섹션에서 자세히 설명 합니다.

### <a name="the-appdelegate-class"></a>AppDelegate 클래스

MonkeyCall는 AppDelegate를 사용 하 여 인스턴스를 보유 하는 `ActiveCallManager` 및 `CXProviderDelegate` 응용 프로그램 전체에서 사용 됨:

```csharp
using Foundation;
using UIKit;
using Intents;
using System;

namespace MonkeyCall
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        #region Constructors
        public override UIWindow Window { get; set; }
        public ActiveCallManager CallManager { get; set; }
        public ProviderDelegate CallProviderDelegate { get; set; }
        #endregion

        #region Override Methods
        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Initialize the call handlers
            CallManager = new ActiveCallManager ();
            CallProviderDelegate = new ProviderDelegate (CallManager);

            return true;
        }

        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            // Get handle from url
            var handle = StartCallRequest.CallHandleFromURL (url);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from URL: {0}", url); 
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
        {
            var handle = StartCallRequest.CallHandleFromActivity (userActivity);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        ...
        #endregion
    }
}
```

`OpenUrl` 및 `ContinueUserActivity` 재정의 앱은 보내는 호출을 처리 하는 경우 메서드가 사용 됩니다. 자세한 내용은 참조 하십시오는 [보내는 호출을 처리](#Handling-Outgoing-Calls) 아래 섹션.

## <a name="handling-incoming-calls"></a>들어오는 호출을 처리합니다.

여러 가지 상태와 수신 VOIP 호출 통과할 수 있는 일반적인 들어오는 호출 워크플로 중와 같은 프로세스 가지가 있습니다.

- 알리는 사용자 (및 시스템)는 들어오는 통화가 있는지 합니다.
- 사용자가을 유용성 될 때 알림을 받을 하 고 다른 사용자를 사용 하 여 호출을 초기화 합니다.
- 사용자가 현재 통화를 종료 하려고 할 때 시스템과 통신 네트워크에 게 알립니다.

다음 섹션에서는 앱 CallKit을 사용 하 여 다시 MonkeyCall VOIP 응용 프로그램을 사용 하 여 예를 들어, 들어오는 호출 워크플로 처리 하는 방법에 대해 자세히를 걸립니다.

### <a name="informing-user-of-incoming-call"></a>들어오는 호출의 사용자에 게 알리지

원격 사용자는 로컬 사용자와 VOIP 대화를 시작 하는 경우 결과 다음과 같습니다.

[![](callkit-images/callkit05.png "원격 사용자가 VOIP 대화를 시작 하는")](callkit-images/callkit05.png#lightbox)

1. 응용 프로그램은 알림을 가져옵니다 해당 통신 네트워크에서 들어오는 VOIP 호출 있다는 것입니다.
2. 응용 프로그램 사용 하 여는 `CXProvider` 보내려고는 `CXCallUpdate` 시스템 호출의 알립니다.
3. 시스템에서 시스템 UI, 시스템 서비스 및 CallKit를 사용 하 여 다른 VOIP 앱에 대 한 호출을 게시 합니다.

예를 들어는 `CXProviderDelegate`:

```csharp
public void ReportIncomingCall (NSUuid uuid, string handle)
{
    // Create update to describe the incoming call and caller
    var update = new CXCallUpdate ();
    update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

    // Report incoming call to system
    Provider.ReportNewIncomingCall (uuid, update, (error) => {
        // Was the call accepted
        if (error == null) {
            // Yes, report to call manager
            CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
        } else {
            // Report error to user here
            Console.WriteLine ("Error: {0}", error);
        }
    });
}
```

이 코드에서는 새 `CXCallUpdate` 인스턴스 되 고 호출자를 식별 하는 것에 대 한 핸들을 첨부 합니다. 사용 하 여 다음으로 `ReportNewIncomingCall` 의 메서드는 `CXProvider` 시스템 호출에 알리기 위해 클래스입니다. 성공한 경우 호출을 추가 하려면 활성 호출 응용 프로그램의 컬렉션, 그렇지 않으면 오류 해야 사용자에 게 보고 합니다.

### <a name="user-answering-incoming-call"></a>사용자 응답 수신 전화

사용자를 수신 VOIP 전화에 답변 하려고 하는 경우 결과 다음과 같습니다.

[![](callkit-images/callkit06.png "사용자는 들어오는 VOIP 호출에 응답")](callkit-images/callkit06.png#lightbox)

1. 시스템 UI VOIP 호출에 응답 하도록 사용자에 게 시스템에 알립니다.
2. 시스템은 보냅니다는 `CXAnswerCallAction` 에 응용 프로그램의 `CXProvider` 응답 의도 알립니다.
3. 응용 프로그램 통신 네트워크 상에 존재 함을 알리고 사용자가 전화를 응답 VOIP 호출 정상적으로 진행 됩니다.

예를 들어는 `CXProviderDelegate`:

```csharp
public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.AnswerCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

이 코드는 먼저 활성 호출 목록에 지정 된 호출에 대 한 검색합니다. 호출을 찾을 수 없는 시스템 알림이 전송 되 고 메서드 종료 됩니다. 발견 되는 경우는 `AnswerCall` 의 메서드는 `ActiveCall` 성공 하거나 실패 하는 경우 시스템은 정보 및 클래스 호출을 시작 하기 위해 호출 됩니다.

### <a name="user-ending-incoming-call"></a>수신 전화를 종료 하는 사용자

응용 프로그램의 UI 내에서 호출에서 종료 하려는 경우 결과 다음과 같습니다.

[![](callkit-images/callkit07.png "사용자 응용 프로그램의 UI 내에서 호출을 종료")](callkit-images/callkit07.png#lightbox)

1. 응용 프로그램을 만듭니다 `CXEndCallAction` 에 번들로 제공 가져옵니다는 `CXTransaction` 호출이 종료 되 게 알리기 위해 시스템에 전송 합니다.
2. 시스템 종료 호출 의도 확인 하 고 보냅니다는 `CXEndCallAction` 를 통해 앱으로 다시는 `CXProvider`합니다.
3. 그런 다음 응용 프로그램 종료 되는 호출 통신 네트워크 상에 존재 알립니다.

예를 들어는 `CXProviderDelegate`:

```csharp
public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.EndCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Remove call from manager's queue
            CallManager.Calls.Remove (call);

            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

이 코드는 먼저 활성 호출 목록에 지정 된 호출에 대 한 검색합니다. 호출을 찾을 수 없는 시스템 알림이 전송 되 고 메서드 종료 됩니다. 발견 되는 경우는 `EndCall` 의 메서드는 `ActiveCall` 성공 하거나 실패 하는 경우 시스템은 정보 및 클래스는 통화를 종료 하기 위해 호출 됩니다. 성공 하면 호출 활성 호출의 컬렉션에서 제거 됩니다.

## <a name="managing-multiple-calls"></a>다중 통화를 관리

대부분의 VOIP 응용 프로그램은 여러 번 호출을 한 번에 처리할 수 있습니다. 예: ˇ ľ ř ˝ 활성 VOIP 통화 및는 임을 새 들어오는 호출을 사용자 앱 가져옵니다 알림 수 일시 중지 하는 경우 또는 끊기 대답 두 번째 식의 첫 번째 호출입니다.

위의 상황 지정에는 시스템에서 보냅니다는 `CXTransaction` 여러 동작의 목록을 포함 하는 응용 프로그램에 (같은 `CXEndCallAction` 및 `CXAnswerCallAction`). 이러한 작업의 모든 값은 시스템 UI를 적절 하 게 업데이트할 수 있도록 개별적으로 수행 될 해야 합니다.

## <a name="handling-outgoing-calls"></a>호출을 나가는 처리

사용자가 휴대폰 앱) (에서 제외할지 목록에서 항목을 하는 경우 예를 들어, 앱에 속하는 호출에서 즉 보내집니다는 _호출 의도 시작_ 시스템에서:

[![](callkit-images/callkit08.png "시작 호출 의도 수신합니다.")](callkit-images/callkit08.png#lightbox)

1. 응용 프로그램을 만듭니다는 _호출 동작 시작_ 시스템에서 받은 시작 호출 의도에 따라 합니다. 
2. 앱이 사용의 `CXCallController` 를 시스템에서 시작 호출 작업을 요청 합니다.
3. 반환을 통해 앱에 됩니다는 시스템에서 작업을 허용 하는 경우는 `XCProvider` 위임 합니다.
4. 응용 프로그램 통신 네트워크 상에 존재로 보내는 호출을 시작합니다.

의도에 대 한 자세한 내용은 참조 하십시오 우리의 [의도 및 의도 UI 확장](~/ios/platform/sirikit/understanding-sirikit.md) 설명서입니다. 

### <a name="the-outgoing-call-lifecycle"></a>나가는 호출 수명 주기

CallKit 및 나가는 호출에서 작업할 때 응용 프로그램 시스템 다음 수명 주기 이벤트에 알리기 위해 필요 합니다.

1. **시작** -나가는 호출을 시작 하려고 하는 시스템에 게 알립니다.
2. **시작** -나가는 호출이 시작 되었음을 시스템에 게 알립니다.
3. **연결** -보내는 호출을 연결 하는 시스템에 게 알립니다.
4. **연결 된** -호출이 연결 하 고 두 당사자 모두가 지금 서로 연결할 수는 나가는 알려야 합니다.

예를 들어 다음 코드는 나가는 호출을 시작 됩니다.

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void StartCall (string contact)
{
    // Build call action
    var handle = new CXHandle (CXHandleType.Generic, contact);
    var startCallAction = new CXStartCallAction (new NSUuid (), handle);

    // Create transaction
    var transaction = new CXTransaction (startCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

만듭니다는 `CXHandle` 사용 하 여 구성는 `CXStartCallAction` 에 버전에 포함 되어는 `CXTransaction` 즉 사용 하 여 시스템에 전송 된 `RequestTransaction` 메서드는 `CXCallController` 클래스. 호출 하 여는 `RequestTransaction` 메서드를 시스템 소스에 관계 없이 모든 기존 호출에서-대기를 배치할 수 (FaceTime, VOIP 전화 앱 등) 새 호출을 시작 하기 전에, 합니다.

나가는 VOIP 호출을 시작 하기 위한 요청 (연락처 앱)에서 대화 상대 카드에 대 한 항목을 Siri 등의 여러 다른 원본 또는 (휴대폰 앱)에서 제외할지 목록에서 가져올 수 있습니다. 이러한 상황에서 응용 프로그램 받게 될 시작 호출 여부는 내부는 `NSUserActivity` 는 AppDelegate 처리 해야 합니다.

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    var handle = StartCallRequest.CallHandleFromActivity (userActivity);

    // Found?
    if (handle == null) {
        // No, report to system
        Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
        return false;
    } else {
        // Yes, start call and inform system
        CallManager.StartCall (handle);
        return true;
    }
}
```

여기에서 `CallHandleFromActivity` 도우미 클래스의 메서드 `StartCallRequest` 호출 되 고 사용자에 게 핸들을 가져오는 데 사용 되 고 (참조 [StartCallRequest 클래스](#The-StartCallRequest-Class) 위에). 

`PerformStartCallAction` 의 메서드는 [ProviderDelegate 클래스](#The-ProviderDelegate-Class) 마지막는 실제 보내는 호출을 시작 하 고 해당 수명 주기 전체 시스템 알리기 하는 데 사용 됩니다.

```csharp
public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
{
    // Create new call record
    var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

    // Monitor state changes
    activeCall.StartingConnectionChanged += (call) => {
        if (call.IsConnecting) {
            // Inform system that the call is starting
            Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNsDate());
        }
    };

    activeCall.ConnectedChanged += (call) => {
        if (call.IsConnected) {
            // Inform system that the call has connected
            Provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNsDate ());
        }
    };

    // Start call
    activeCall.StartCall ((successful) => {
        // Was the call able to be started?
        if (successful) {
            // Yes, inform the system
            action.Fulfill ();

            // Add call to manager
            CallManager.Calls.Add (activeCall);
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

인스턴스를 만듭니다는 `ActiveCall` (정보를 보관 하는 호출에 대 한 진행 중에서) 클래스 및 호출 되 고 사용자와 정보를 표시 합니다. `StartingConnectionChanged` 및 `ConnectedChanged` 이벤트 모니터링 하 고 나가는 호출 수명 주기를 보고 하는 데 사용 됩니다. 호출 시작 되 하 고 시스템 작업이 수행 되었음을 정보.

### <a name="ending-an-outgoing-call"></a>나가는 호출을 종료

사용자를 종료 하려고 한다고와 나가는 호출 완료 되 면 다음 코드를 사용할 수 있습니다.

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void EndCall (ActiveCall call)
{
    // Build action
    var endCallAction = new CXEndCallAction (call.UUID);

    // Create transaction
    var transaction = new CXTransaction (endCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

경우 만듭니다는 `CXEndCallAction` 종료에 대 한 호출의 UUID와 함께 묶는는 `CXTransaction` 즉 사용 하 여 시스템에 전송는 `RequestTransaction` 의 메서드는 `CXCallController` 클래스입니다. 

## <a name="additional-callkit-details"></a>추가 CallKit 세부 정보

이 섹션에서는 개발자와 같은 CallKit 작업할 때는 고려해 해야 하는 추가 세부 정보를 설명 합니다.

- 공급자 구성
- 작업 오류
- 시스템 제한 사항
- VOIP 오디오

### <a name="provider-configuration"></a>공급자 구성

공급자 구성을 통해 때 (내부의 네이티브 호출에서는 UI) 사용자 환경을 사용자 지정 하는 iOS 10 VOIP 응용 프로그램 CallKit 사용 합니다.

앱 다음 유형의 사용자 지정을 수행할 수 있습니다.

- 지역화 된 이름을 표시 합니다.
- 비디오 호출 지원할을 수 있습니다.
- In 호출 UI의 단추를 마스크 된 이미지 아이콘을 제공 하 여 사용자 지정 합니다. 사용자 지정 단추가 포함 된 사용자 상호 작용 처리 해야 하는 응용 프로그램에 직접 보내집니다. 

### <a name="action-errors"></a>작업 오류

iOS 10 VOIP 앱 CallKit를 사용 하 여 안전한 오류 동작을 처리 하 고 모든 시간에 작업 상태에 대 한 안내 하는 사용자를 유지 해야 합니다. 

다음 예제에서는 고려해에 두십시오.

1. 응용 프로그램 시작 호출 작업을이 해당 통신 네트워크와 새 VOIP 호출을 초기화 하는 프로세스를 시작 합니다.
2. 제한 된 또는 없습니다 네트워크 통신 기능으로 인해이 연결이 실패합니다.
3. 응용 프로그램 *해야* 보내기는 **실패** 게 메시지를 다시 시작 호출 작업 (`Action.Fail()`) 시스템 오류에 알리기 위해 합니다.
4. 이 통해 시스템 호출의 상태를 사용자에 게 알리는 수입니다. 예를 들어 호출 실패 UI를 표시 합니다.

IOS 10 VOIP 앱에 응답 하도록 해야 합니다는 또한 _시간 초과 오류_ 예상된 작업 시간가 주어진된 시간 내에서 처리할 수 없을 때 발생할 수 있는 합니다. CallKit에서 제공 하는 각 작업 유형에 관련 된 최대 제한 시간 값을 갖습니다. 이 시간 제한 값 사용자가 요청한 CallKit 조치 처리 하는 응답성이 뛰어난 방식으로 유지할 OS 유동성 및 응답성이 확인 합니다.

여러 가지 방법을 공급자 대리자에서 (`CXProviderDelegate`)를 정상적으로이 시간 초과 상황을 처리할 재정의 해야 합니다.

### <a name="system-restrictions"></a>시스템 제한 사항

IOS 10 VOIP 응용 프로그램을 실행 하는 iOS 장치의 현재 상태에 따라, 특정 시스템 제한이 적용 될 수 있습니다.

예를 들어 들어오는 VOIP 호출 하는 경우 시스템에 의해 제한 될 수 있습니다.

1. 호출 하는 사용자는 사용자의 차단 호출자 목록 켜져 있습니다.
2. IOS 장치 사용자의 Do Not 방해 모드입니다.

VOIP 전화는 이러한 상황에 의해 제한, 취급 하는 데 다음 코드를 사용 합니다.

```csharp
public class ProviderDelegate : CXProviderDelegate
{
...

    public void ReportIncomingCall (NSUuid uuid, string handle)
    {
        // Create update to describe the incoming call and caller
        var update = new CXCallUpdate ();
        update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);
    
        // Report incoming call to system
        Provider.ReportNewIncomingCall (uuid, update, (error) => {
            // Was the call accepted
            if (error == null) {
                // Yes, report to call manager
                CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
            } else {
                // Report error to user here
                if (error.Code == (int)CXErrorCodeIncomingCallError.CallUuidAlreadyExists) {
                    // Handle duplicate call ID
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByBlockList) {
                    // Handle call from blocked user
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByDoNotDisturb) {
                    // Handle call while in do-not-disturb mode
                } else {
                    // Handle unknown error
                }
            }
        });
    }

}
```

### <a name="voip-audio"></a>VOIP 오디오

CallKit 라이브 VOIP 호출을 사용 하는 동안 iOS 10 VOIP 앱 해야 하는 오디오 리소스를 처리 하기 위한 여러 가지 이점을 제공 합니다. 응용 프로그램의 가장 큰 이점 중 하나는 오디오 세션은 더 높은 우선 순위 10 iOS에서 실행 하는 경우. 이 세션은 전화에서 기본 제공으로 동일한 우선 순위 수준 및 FaceTime 앱과이 향상 된 우선 순위 수준을 실행 중인 다른 응용 프로그램에서 VOIP 응용 프로그램의 오디오 세션을 중단 합니다.

또한 CallKit에 성능을 향상 하 고 특정 출력 장치에 사용자 기본 설정 및 장치 상태에 따라 라이브 호출 하는 동안 VOIP 오디오를 지능적으로 라우트할 수 있는 다른 오디오 라우팅 힌트에 대 한 액세스. 예를 들어 라이브 CarPlay 연결 또는 내게 필요한 옵션 설정 bluetooth 헤드폰 등 연결 된 장치 기반으로 합니다.

일반적인 VOIP의 수명 주기 동안 CallKit를 사용 하 여 호출, 응용 프로그램 오디오 스트림을 CallKit를 제공할 것을 구성 해야 합니다. 다음 예제를 살펴보겠습니다.

[![](callkit-images/callkit09.png "시작 호출 작업 시퀀스")](callkit-images/callkit09.png#lightbox)

1. 호출 작업은 들어오는 호출에 응답 하도록 응용 프로그램에서 수신 됩니다.
2. 이 작업, 응용 프로그램에서 처리 되기 전에 구성에 필요한 제공 해당 `AVAudioSession`합니다.
3. 응용 프로그램 작업이 수행 된 시스템에 알립니다.
4. CallKit 높은 우선 순위의 제공 호출 연결 하기 전에 `AVAudioSession` 응용 프로그램 요청 하는 구성과 일치 하는 합니다. 응용 프로그램을 통해 알림이 표시 됩니다는 `DidActivateAudioSession` 방식의 해당 `CXProviderDelegate`합니다.

## <a name="working-with-call-directory-extensions"></a>호출 디렉터리 확장 사용

CallKit, 작업할 때 _디렉터리 확장 호출_ 차단 된 전화 번호를 추가 하 고 iOS 장치에서 연락처 앱에 있는 연락처를 지정된 된 VOIP 앱에만 적용 되는 번호를 식별 하는 방법을 제공 합니다.

### <a name="implementing-a-call-directory-extension"></a>호출 디렉터리 확장 프로그램 구현

Xamarin.iOS 앱에서 호출 디렉터리 확장을 구현 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac.에 대 한 Visual Studio에서 응용 프로그램의 솔루션을 열으십시오
2. 솔루션 이름을 마우스 오른쪽 단추로 클릭는 **솔루션 탐색기** 선택 **추가** > **새 프로젝트 추가**합니다.
3. 선택 **iOS** > **확장** > **디렉터리 확장 호출** 클릭는 **다음** 단추: 

    [![](callkit-images/calldir01.png "새 호출 디렉터리 확장 만들기")](callkit-images/calldir01.png#lightbox)
4. 입력 한 **이름** 확장과 클릭에 대 한는 **다음** 단추: 

    [![](callkit-images/calldir02.png "확장에 대 한 이름을 입력합니다.")](callkit-images/calldir02.png#lightbox)
5. 조정 된 **프로젝트 이름** 및/또는 **솔루션 이름** 필요 하 고 클릭는 **만들기** 단추: 

    [![](callkit-images/calldir03.png "프로젝트 만들기")](callkit-images/calldir03.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio에서 응용 프로그램의 솔루션을 엽니다.
2. 솔루션 이름을 마우스 오른쪽 단추로 클릭는 **솔루션 탐색기** 선택 **추가** > **새 프로젝트 추가**합니다.
3. 선택 **iOS** > **확장** > **디렉터리 확장 호출** 클릭는 **다음** 단추: 

    [![](callkit-images/calldir01w.png "새 호출 디렉터리 확장 만들기")](callkit-images/calldir01.png#lightbox)
4. 입력 한 **이름** 확장과 클릭에 대 한는 **확인** 단추

-----


이렇게 하면 추가 됩니다 한 `CallDirectoryHandler.cs` 다음과 같이 하는 프로젝트에 클래스:

```csharp
using System;

using Foundation;
using CallKit;

namespace MonkeyCallDirExtension
{
    [Register ("CallDirectoryHandler")]
    public class CallDirectoryHandler : CXCallDirectoryProvider, ICXCallDirectoryExtensionContextDelegate
    {
        #region Constructors
        protected CallDirectoryHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void BeginRequest (CXCallDirectoryExtensionContext context)
        {
            context.Delegate = this;

            if (!AddBlockingPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add blocking phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 1, null);
                context.CancelRequest (error);
                return;
            }

            if (!AddIdentificationPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add identification phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 2, null);
                context.CancelRequest (error);
                return;
            }

            context.CompleteRequest (null);
        }
        #endregion

        #region Private Methods
        private bool AddBlockingPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to block from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 14085555555, 18005555555 };

            foreach (var phoneNumber in phoneNumbers)
                context.AddBlockingEntry (phoneNumber);

            return true;
        }

        private bool AddIdentificationPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to identify and their identification labels from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 18775555555, 18885555555 };
            string [] labels = { "Telemarketer", "Local business" };

            for (var i = 0; i < phoneNumbers.Length; i++) {
                long phoneNumber = phoneNumbers [i];
                string label = labels [i];
                context.AddIdentificationEntry (phoneNumber, label);
            }

            return true;
        }
        #endregion

        #region Public Methods
        public void RequestFailed (CXCallDirectoryExtensionContext extensionContext, NSError error)
        {
            // An error occurred while adding blocking or identification entries, check the NSError for details.
            // For Call Directory error codes, see the CXErrorCodeCallDirectoryManagerError enum.
            //
            // This may be used to store the error details in a location accessible by the extension's containing app, so that the
            // app may be notified about errors which occurred while loading data even if the request to load data was initiated by
            // the user in Settings instead of via the app itself.
        }
        #endregion
    }
}
```

`BeginRequest` 디렉터리 호출 처리기에서 메서드를 필요한 기능을 제공 하도록 수정 해야 합니다. 위의 샘플의 경우와 사용 가능한 번호 목록이 VOIP 응용 프로그램의 연락처 데이터베이스에 설정 하려고 합니다. 어떤 이유로 든 실패를 요청 하거나, 만듭니다는 `NSError` 전달 고 오류를 설명 하는 `CancelRequest` 의 메서드는 `CXCallDirectoryExtensionContext` 클래스입니다.

차단 된 번호가 사용을 설정 하는 `AddBlockingEntry` 의 메서드는 `CXCallDirectoryExtensionContext` 클래스입니다. 메서드에 제공 된 숫자 _해야_ 숫자로 오름차순 이어야 합니다. 최적의 성능 및 메모리 사용에 대 한 전화 번호를 여러 사용자가 경우 고려만 지정된 된 시간에 숫자의 하위 집합을 로드 하 고 autorelease 풀을 사용 하 여 로드 되는 숫자의 각 일괄 처리 중에 할당 된 개체를 해제 하 합니다.

VOIP 응용 프로그램에 알려진 연락처 번호의 연락처 앱에 게 알리기 위해 사용 하 여는 `AddIdentificationEntry` 의 메서드는 `CXCallDirectoryExtensionContext` 클래스 및 수와 식별 레이블로 제공 합니다. 메서드에 제공 된 숫자 다시 _해야_ 숫자로 오름차순 이어야 합니다. 최적의 성능 및 메모리 사용에 대 한 전화 번호를 여러 사용자가 경우 고려만 지정된 된 시간에 숫자의 하위 집합을 로드 하 고 autorelease 풀을 사용 하 여 로드 되는 숫자의 각 일괄 처리 중에 할당 된 개체를 해제 하 합니다.


## <a name="summary"></a>요약

이 문서는 새로운 CallKit API 해당 Apple iOS 10과 Xamarin.iOS VOIP 응용 프로그램에서 구현 하는 방법에서 릴리스 검사가 수행 됩니다. CallKit iOS 시스템에 통합 하는 응용 프로그램을 허용 하는 방법을 어떻게 기본 제공 앱 (예: 전화)과 기능 패리티를 제공 하 고 iOS Siri 상호 작용을 통해 및 via 잠금 및 홈 화면을 같은 위치에 대 한 시야는 응용 프로그램의 증가 방식 나타났습니다. 연락처 앱입니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
