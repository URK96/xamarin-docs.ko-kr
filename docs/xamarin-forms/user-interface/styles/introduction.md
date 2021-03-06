---
title: 스타일 소개
description: 스타일 모양을 사용자 지정할 수 있는 시각적 요소의 허용 합니다. 스타일 특정 형식에 대해 정의 및 해당 형식에 사용할 수 있는 속성에 대 한 값을 포함 합니다.
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 453c4d6edafd6493272f8ca0435fcc86e2f3b2f7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-styles"></a>스타일 소개

_스타일 모양을 사용자 지정할 수 있는 시각적 요소의 허용 합니다. 스타일 특정 형식에 대해 정의 및 해당 형식에 사용할 수 있는 속성에 대 한 값을 포함 합니다._

Xamarin.Forms 응용 프로그램에 동일한 모양을 갖도록 하는 여러 컨트롤 포함 되는 경우가 많습니다. 예를 들어 응용 프로그램이 여러 있을 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 다음 XAML 코드 예제와 같이 동일한 글꼴 옵션 및 레이아웃 옵션을 포함 하는 인스턴스:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Styles.NoStylesPage"
    Title="No Styles"
    Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="are not"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="using styles"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

다음 코드 예제에서는 C#에서 만들어진 해당 페이지를 보여 줍니다.

```csharp
public class NoStylesPageCS : ContentPage
{
    public NoStylesPageCS ()
    {
        Title = "No Styles";
        Icon = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label {
                    Text = "These labels",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "are not",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "using styles",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                }
            }
        };
    }
}
```

각 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 인스턴스에 동일한 속성 값으로 표시 되는 텍스트의 모양을 제어 하기 위한는 `Label`합니다. 다음 스크린샷에 표시 된 모양 결과이 됩니다.

[![](introduction-images/no-styles.png "스타일 모양에 레이블을")](introduction-images/no-styles-large.png#lightbox "스타일 모양에 레이블을")

반복 될 수 있습니다 각 개별 컨트롤의 모양을 설정 하 고 오류가 발생 하기 쉽습니다. 대신, 스타일을 만들 수 있습니다 하의 모양을 정의 하 고 그런 다음 필요한 컨트롤에 적용 합니다.

## <a name="creating-a-style"></a>스타일 만들기

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 다음 여러 시각적 요소 인스턴스에 적용할 수 있는 하나의 개체에 속성 값의 컬렉션을 그룹화 하는 클래스입니다. 이 반복적으로 표시 되는 경우를 줄일 수 있습니다 및 응용 프로그램 모양을 보다 쉽게 변경할 수 있습니다.

스타일 주로 XAML 기반 응용 프로그램에 디자인에 있지만 만들 수도 있습니다 C#에서:

- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) XAML에서 생성 된 인스턴스는 일반적으로 정의 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 에 할당 된는 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) 컬렉션 컨트롤의 페이지 또는 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) 응용 프로그램의 컬렉션입니다.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) C#에서 만든 인스턴스는 일반적으로 페이지의 클래스 또는 전체적으로 액세스할 수 있는 클래스에서 정의 됩니다.

정의할 위치를 선택는 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 사용할 수 있는 위치에 영향을 미칩니다.

- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 제어 수준에 정의 된 인스턴스 및 해당 자식 컨트롤에 적용할 수 있습니다.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 페이지 수준에서 정의 된 인스턴스 및 해당 자식 페이지에 적용할 수 있습니다.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 응용 프로그램에 걸쳐 응용 프로그램 수준에서 정의 된 인스턴스를 적용할 수 있습니다.

각 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 하나 이상의 컬렉션을 포함 하는 인스턴스 [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) 개체를 `Setter` 필요는 [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) 및 [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/). `Property` 에 스타일이 적용 하는 요소의 바인딩 가능한 속성의 이름 및 `Value` 속성에 적용 되는 값입니다.

각 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 인스턴스 수 *명시적*, 또는 *암시적*:

- *명시적* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 인스턴스를 지정 하 여 정의 되는 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) 및 `x:Key` 값을 복사한 대상 요소 설정[ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 속성을는 `x:Key` 참조 합니다. 에 대 한 자세한 내용은 *명시적* 스타일 참조 [명시적 스타일](~/xamarin-forms/user-interface/styles/explicit.md)합니다.
- *암시적* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 인스턴스가 지정 하 여 정의 된 한 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/)합니다. `Style` 인스턴스 자동으로 해당 형식의 모든 요소에 적용 한 다음 됩니다. 참고의 해당 하위 클래스는 `TargetType` 자동으로 제공 되지 않습니다는 `Style` 적용 합니다. 에 대 한 자세한 내용은 *암시적* 스타일 참조 [암시적 스타일](~/xamarin-forms/user-interface/styles/implicit.md)합니다.

만들 때 한 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/), [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) 속성은 항상 필요 합니다. 다음 코드 예제는 *명시적* 스타일 (참고는 `x:Key`) XAML에서 만든:

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

적용할는 `Style`, 대상 개체가 있어야는 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) 와 일치 하는 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) 의 속성 값은 `Style`다음 XAML 코드 예제에 나온 것 처럼:

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

아래쪽 계층 구조 보기에 있는 스타일을 높을수록 정의 된 보다 우선 합니다. 예를 들어 설정는 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 설정 하는 [ `Label.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) 를 `Red` 응용 프로그램에서 수준으로 설정 하는 페이지 수준 스타일 재정의 됩니다 `Label.TextColor` 를`Green`. 마찬가지로, 페이지 수준 스타일 제어 수준 스타일에 의해 재정의 됩니다. 또한 경우 `Label.TextColor` 직접에서 컨트롤 속성을이 우선 스타일이 모두 설정 되어 있습니다.

이 섹션의에서 문서를 보여 주고 만들고 적용 하는 방법을 설명 *명시적* 및 *암시적* 스타일 글로벌 스타일을 만들고, 스타일을 상속 하는 방법을 런타임 시, 스타일 변경 내용에 응답 하는 방법 및 Xamarin.Forms에 포함 된 기본 제공 스타일을 사용 하는 방법을 지정 합니다.

> [!NOTE]
> **StyleId 란?**
>
> Xamarin.Forms 2.2 이전는 [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) 속성은 Pixate 같은 테마 엔진 및 UI 테스트의 id에 대 한 응용 프로그램의 개별 요소를 식별 하는 데 사용 되었습니다. Xamarin.Forms 2.2에 도입 하는 반면는 [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/) 는 대체 속성의 [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) 속성입니다. 자세한 내용은 참조 [Xamarin.UITest 및 테스트 클라우드를 사용 하 여 테스트를 자동화 하는 Xamarin.Forms](~/xamarin-forms/deploy-test/uitest-and-test-cloud.md)합니다.

## <a name="summary"></a>요약

Xamarin.Forms 응용 프로그램에 동일한 모양을 갖도록 하는 여러 컨트롤 포함 되는 경우가 많습니다. 반복 될 수 있습니다 각 개별 컨트롤의 모양을 설정 하 고 오류가 발생 하기 쉽습니다. 대신, 스타일 여 만들 수 있습니다 컨트롤 모양을 사용자 지정 하는 그룹화 및 설정 속성 컨트롤 형식에 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Style](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
