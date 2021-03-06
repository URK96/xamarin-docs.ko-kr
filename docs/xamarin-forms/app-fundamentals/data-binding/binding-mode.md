---
title: 바인딩 모드
description: 원본과 대상 간의 정보 흐름을 제어 합니다.
ms.prod: xamarin
ms.assetid: D087C389-2E9E-47B9-A341-5B14AC732C45
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: fdcba9b680bd548371883788af9e4eda755d91df
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="binding-mode"></a>바인딩 모드

에 [이전 문서](basic-bindings.md), **대체 코드 바인딩** 및 **대신 XAML 바인딩** 갖춘 페이지는 `Label` 와 해당 `Scale` 속성 에 바인딩된는 `Value` 속성은 `Slider`합니다. 때문에 `Slider` 초기 값은 0, 때문일는 `Scale` 속성의는 `Label` 1이 아닌 0으로 설정 됩니다 및 `Label` 사라졌습니다.

에 [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) 샘플에서는 **역방향 바인딩** 페이지는 이전 문서에서 프로그램으로 제외 하 고 에데이터바인딩이정의`Slider` 대신에 `Label`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.ReverseBindingPage"
             Title="Reverse Binding">
    <StackLayout Padding="10, 0">

        <Label x:Name="label" 
               Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                VerticalOptions="CenterAndExpand"
                Value="{Binding Source={x:Reference label},
                                Path=Opacity}" />
    </StackLayout>
</ContentPage>
```

처음에이 것 처럼 보일 수 이전 버전과: 이제는 `Label` 원본인 데이터 바인딩 및 `Slider` 대상입니다. 바인딩 참조는 `Opacity` 의 속성은 `Label`에 1의 기본 값이 있는 합니다.

예상 대로 `Slider` 에서 초기 값을 1로 초기화 `Opacity` 값 `Label`합니다. 이 iOS 스크린샷에서 왼쪽에 표시 됩니다.

[![바인딩 역방향](binding-mode-images/reversebinding-small.png "바인딩 역방향")](binding-mode-images/reversebinding-large.png#lightbox "역방향 바인딩")

하지만 수 있습니다는 달라진는 `Slider` Android 및 UWP 스크린 샷을 설명한 것 처럼 작동 하도록 하는 계속 됩니다. 이 방법은 데이터 바인딩은 더 나은 경우에 작동 하는지 제안 하는 `Slider` 은 바인딩 대상 보다는 `Label` 기대할 수와 같은 초기화는 작동 하기 때문에 있습니다.

차이 **역방향 바인딩** 샘플과 이전 샘플에 포함 됩니다는 *바인딩 모드*합니다.

## <a name="the-default-binding-mode"></a>기본 바인딩 모드

멤버와 지정 된 바인딩 모드는 [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/) 열거형: 

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.Default/) 
- [`TwoWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) &ndash; 데이터가 원본과 대상 간의 양방향 이동
- [`OneWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWay/) &ndash; 데이터 소스에서 대상으로 이동
- [`OneWayToSource`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) &ndash; 데이터 소스에 대상에서 이동합니다.

모든 바인딩 가능한 속성에는 기본 바인딩 가능 속성 만들어질 때 설정 되는 모드를 바인딩 및에서 사용할 수는 [ `DefaultBindingMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableProperty.DefaultBindingMode/) 의 속성은 `BindableProperty` 개체입니다. 이 기본 바인딩 모드 속성은 데이터 바인딩 대상 때 적용 모드를 나타냅니다.

와 같은 대부분의 속성에 대 한 기본 바인딩 모드 `Rotation`, `Scale`, 및 `Opacity` 은 `OneWay`합니다. 이러한 속성을 대상으로 데이터 바인딩 소스에서 대상 속성이 설정 됩니다.

그러나에 대 한 기본 바인딩 모드는 `Value` 속성 `Slider` 은 `TwoWay`합니다. 즉, 해당 경우에는 `Value` 속성은 데이터 바인딩 대상 다음 대상이 소스에서 (평소와 같이) 설정 되어 있지만 대상에서 소스도 설정 됩니다. 이 통해는 `Slider` 초기에서 `Opacity` 값입니다.

이 양방향 바인딩에 무한 루프를 만드는 것 처럼 보일 수 있지만 발생 하지 않습니다. 속성이 실제로 변경 하지 않는 바인딩 가능 속성 속성이 변경 된 신호를 보내지 않습니다. 이렇게 하면 무한 루프가 않습니다.

### <a name="two-way-bindings"></a>양방향 바인딩으로 설정

가장 바인딩 가능한 속성의 기본 바인딩 모드에는 `OneWay` 다음 속성의 기본 바인딩 모드에는 있지만 `TwoWay`:

- `Date` 속성 `DatePicker`
- `Text` 속성의 `Editor`, `Entry`, `SearchBar`, 및 `EntryCell`
- `IsRefreshing` 속성 `ListView`
- `SelectedItem` 속성 `MultiPage`
- `SelectedIndex` 및 `SelectedItem` 의 속성 `Picker`
- `Value` 속성의 `Slider` 및 `Stepper`
- `IsToggled` 속성 `Switch` 
- `On` 속성 `SwitchCell`
- `Time` 속성 `TimePicker`

이러한 특정 속성으로 정의 된 `TwoWay` 매우 좋은 이유: 

ViewModel 클래스는 데이터 바인딩 소스 및 보기와 같은 보기의 구성 된 데이터 바인딩 모델-뷰-MVVM () 응용 프로그램 아키텍처와 함께 사용 되는 경우 `Slider`, 데이터 바인딩 대상입니다. MVVM 바인딩 유사는 **바인딩 역방향** 이전 샘플에서 바인딩 보다 더 많은 샘플입니다. 각 페이지의 보기에는 ViewModel에서 해당 속성의 값으로 초기화 하지만 보기에서 변경 내용을 ViewModel 속성에는 영향을 줄은 매우 높습니다.

기본 바인딩 모드를 사용 하 여 속성 `TwoWay` 는 MVVM 시나리오에서 사용할 가능성이 가장 높은 속성입니다.

### <a name="one-way-to-source-bindings"></a>한-단방향-에-소스 바인딩

읽기 전용 바인딩 가능한 속성의 기본 바인딩 모드에는 `OneWayToSource`합니다. 기본 바인딩 모드에 있는 하나의 읽기/쓰기 바인딩 가능한 속성은 `OneWayToSource`:

- `SelectedItem` 속성 `ListView`

도입한 이유는 바인딩의 `SelectedItem` 속성을 설정 하 여 바인딩 소스 유발 합니다. 이 문서 뒷부분의 예는 해당 동작을 재정의합니다.

## <a name="viewmodels-and-property-change-notifications"></a>Viewmodel 및 속성이 변경 알림

**간단한 색 선택기** 페이지 간단한 ViewModel의 사용법을 보여줍니다. 데이터 바인딩에서는 3 개를 사용 하 여 색을 선택 하면 `Slider` 색상, 채도 및 명도 대 한 요소입니다.

ViewModel에 데이터 바인딩 소스입니다. ViewModel 않습니다 *하지* 바인딩 가능 속성을 정의 하지만 바인딩 인프라는 속성의 값이 변경 될 때 알림을 받을 수 있도록 알림 메커니즘을 구현 하는 것 않습니다. 이 알림 메커니즘은는 [ `INotifyPropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) 이라는 단일 속성을 정의 하는 인터페이스 [ `PropertyChanged` ](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/)합니다. 일반적으로이 인터페이스를 구현 하는 클래스의 public 속성 중 하나에 값을 변경할 때 이벤트를 발생 시킵니다. 이벤트 속성 바뀌지 경우 실행 될 필요는 없습니다. (의 `INotifyPropertyChanged` 가 인터페이스를 구현도 `BindableObject` 및 `PropertyChanged` 바인딩 가능한 속성 값이 변경 될 때마다 이벤트가 발생 합니다.)

`HslColorViewModel` 5 개의 속성을 정의 하는 클래스:는 `Hue`, `Saturation`, `Luminosity`, 및 `Color` 속성은 서로 관련 되어 있습니다. 세 가지 중 하나가 색상 구성 요소 변경 값 때는 `Color` 속성 다시 계산 되므로 및 `PropertyChanged` 모든 네 가지 속성에 대 한이 이벤트는 발생:

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name; 

    public event PropertyChangedEventHandler PropertyChanged;

    public double Hue
    {
        set
        {
            if (color.Hue != value)
            {
                Color = Color.FromHsla(value, color.Saturation, color.Luminosity);
            }
        }
        get 
        {
            return color.Hue;
        }
    }

    public double Saturation
    {
        set
        {
            if (color.Saturation != value)
            {
                Color = Color.FromHsla(color.Hue, value, color.Luminosity);
            }
        }
        get
        {
            return color.Saturation;
        }
    }

    public double Luminosity
    {
        set
        {
            if (color.Luminosity != value)
            {
                Color = Color.FromHsla(color.Hue, color.Saturation, value);
            }
        }
        get
        {
            return color.Luminosity;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Hue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Saturation"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Luminosity"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));

                Name = NamedColor.GetNearestColorName(color);
            }
        }
        get
        {
            return color;
        }
    }

    public string Name
    {
        private set
        {
            if (name != value)
            {
                name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Name"));
            }
        }
        get
        {
            return name;
        }
    }
}
```

때는 `Color` 속성 변경, 정적 `GetNearestColorName` 에서 메서드는 `NamedColor` 클래스 (에 포함는 **DataBindingDemos** 솔루션) 가장 가까운 명명 된 색을 가져오고 설정 하는 `Name` 속성입니다. 이 `Name` 속성에는 개인 `set` 접근자를 클래스 외부에서 설정할 수 없습니다.

바인딩 인프라에 처리기를 연결 된 ViewModel 바인딩 소스 개체로 설정 된 경우는 `PropertyChanged` 이벤트입니다. 이러한 방식으로 바인딩 속성을 알림을 받을 수와 그런 다음 대상 속성에서 변경 된 값에서 설정할 수 있습니다.

**간단한 색 선택기** XAML 파일에서 인스턴스화하는 `HslColorViewModel` 페이지의 리소스 사전 및 초기화에는 `Color` 속성입니다. `BindingContext` 의 속성은 `Grid` 로 설정 되는 `StaticResource` 바인딩 해당 리소스를 참조 하는 확장:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SimpleColorSelectorPage">

    <ContentPage.Resources>
        <ResourceDictionary>
            <local:HslColorViewModel x:Key="viewModel" 
                                     Color="MediumTurquoise" />

            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
        
    <Grid BindingContext="{StaticResource viewModel}">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <BoxView Color="{Binding Color}"
                 Grid.Row="0" />

        <StackLayout Grid.Row="1"
                     Margin="10, 0">

            <Label Text="{Binding Name}"
                   HorizontalTextAlignment="Center" />

            <Slider Value="{Binding Hue}" />
    
            <Slider Value="{Binding Saturation}" />

            <Slider Value="{Binding Luminosity}" />
        </StackLayout>
    </Grid>
</ContentPage>
```

`BoxView`, `Label`, 및 세 개의 `Slider` 에서 바인딩 컨텍스트를 상속 하는 뷰는 `Grid`합니다. 이러한 뷰는 원본 속성을 ViewModel 참조 하는 모든 바인딩 대상입니다. 에 대 한는 `Color` 속성은 `BoxView`, 및 `Text` 속성은 `Label`, 데이터 바인딩은 `OneWay`: ViewModel 속성에서 보기에는 속성이 설정 됩니다.

`Value` 의 속성은 `Slider`, 인데 `TwoWay`합니다. 이렇게 하면 각 `Slider` ViewModel에서 및를 각각 설정할 ViewModel 설정할 `Slider`합니다. 

프로그램을 처음 실행할 때의 `BoxView`, `Label`, 3 및 `Slider` 요소는 초기에 따라 ViewModel에서 모두 설정 `Color` 속성이 ViewModel를 인스턴스화할 때 설정 합니다. 이 iOS 스크린샷에서 왼쪽에 표시 됩니다.

[![간단한 색 선택기](binding-mode-images/simplecolorselector-small.png "간단한 색 선택기")](binding-mode-images/simplecolorselector-large.png#lightbox "간단한 색 선택기")

슬라이더를 조작할 때는 `BoxView` 및 `Label` Android 및 UWP 스크린샷을 보려면 표시 된 것 처럼 따라서 업데이트 됩니다.

리소스 사전에서 ViewModel 인스턴스화 일반적인 방법 중 하나입니다. 에 대 한 속성 요소 태그 내의 ViewModel 인스턴스화할 수 이기도 `BindingContext` 속성입니다. 에 **간단한 색 선택기** XAML 파일에서 제거는 `HslColorViewModel` 리소스 사전에서로 설정 하 고는 `BindingContext` 속성은 `Grid` 다음과 같이:

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>
        
    ···

</Grid>
```

여러 가지 방법으로 바인딩 컨텍스트를 설정할 수 있습니다. 경우에 따라 코드 숨김 파일 ViewModel 인스턴스화하고로 설정 된 `BindingContext` 페이지의 속성입니다. 이들은 모두 유효한 접근 방식입니다.

## <a name="overriding-the-binding-mode"></a>바인딩 모드를 재정의합니다.

대상 속성에 기본 바인딩 모드, 특정 데이터 바인딩에 대 한 적합 한 없으면는 설정 하 여 재정의할 수는 [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/) 속성 `Binding` (또는 [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Mode/) 의 속성은 `Binding` 태그 확장)의 멤버 중 하나에 `BindingMode` 열거형입니다.

그러나 설정 된 `Mode` 속성을 `TwoWay` 예상 대로 작동 하지 않습니다. 예를 들어 수정 해 보세요는 **대신 XAML 바인딩** XAML 파일 포함을 `TwoWay` 바인딩 정의에:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

예상 되는 `Slider` 의 초기 값으로 초기화 합니다는 `Scale` 속성을 1, /features 이지만 발생 하지 않습니다. 경우는 `TwoWay` 바인딩을 초기화, 대상은 소스에서 먼저 설정, 즉는 `Scale` 속성이로 설정 되는 `Slider` 기본 0 값입니다. 경우는 `TwoWay` 바인딩은에 설정 됩니다는 `Slider`, 하면 `Slider` 소스에서 처음 설정 됩니다.

바인딩 모드 설정할 수 있습니다 `OneWayToSource` 에 **대신 XAML 바인딩** 샘플:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=OneWayToSource}" />
```

이제는 `Slider` 1로 초기화 됩니다 (의 기본값 `Scale`) 하지만 조작 하기는 `Slider` 영향을 주지 않습니다는 `Scale` 속성,이 기능은 매우 유용 합니다.

기본 바인딩 모드와 재정의의 매우 유용한 응용 프로그램 `TwoWay` 포함 됩니다는 `SelectedItem` 속성 `ListView`합니다. 기본 바인딩 모드는 `OneWayToSource`합니다. 에 데이터 바인딩이 설정 된 경우는 `SelectedItem` 속성에서 해당 소스 속성은 설정는 ViewModel에서 source 속성을 참조 하는 `ListView` 선택 합니다. 그러나 일부 경우에 수도 있습니다는 `ListView` ViewModel에서 초기화 합니다.

**샘플 설정을** 페이지에이 기술을 보여 줍니다. 이 페이지는 종종 매우 이와 같은 ViewModel에 정의 된 응용 프로그램 설정의 간단한 구현을 나타냅니다 `SampleSettingsViewModel` 파일:

```csharp
public class SampleSettingsViewModel : INotifyPropertyChanged
{
    string name;
    DateTime birthDate;
    bool codesInCSharp;
    double numberOfCopies;
    NamedColor backgroundNamedColor;

    public event PropertyChangedEventHandler PropertyChanged;

    public SampleSettingsViewModel(IDictionary<string, object> dictionary)
    {
        Name = GetDictionaryEntry<string>(dictionary, "Name");
        BirthDate = GetDictionaryEntry(dictionary, "BirthDate", new DateTime(1980, 1, 1));
        CodesInCSharp = GetDictionaryEntry<bool>(dictionary, "CodesInCSharp");
        NumberOfCopies = GetDictionaryEntry(dictionary, "NumberOfCopies", 1.0);
        BackgroundNamedColor = NamedColor.Find(GetDictionaryEntry(dictionary, "BackgroundNamedColor", "White"));
    }

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public DateTime BirthDate
    {
        set { SetProperty(ref birthDate, value); }
        get { return birthDate; }
    }

    public bool CodesInCSharp
    {
        set { SetProperty(ref codesInCSharp, value); }
        get { return codesInCSharp; }
    }

    public double NumberOfCopies
    {
        set { SetProperty(ref numberOfCopies, value); }
        get { return numberOfCopies; }
    }

    public NamedColor BackgroundNamedColor
    {
        set
        {
            if (SetProperty(ref backgroundNamedColor, value))
            {
                OnPropertyChanged("BackgroundColor");
            }
        }
        get { return backgroundNamedColor; }
    }

    public Color BackgroundColor
    {
        get { return BackgroundNamedColor?.Color ?? Color.White; }
    }

    public void SaveState(IDictionary<string, object> dictionary)
    {
        dictionary["Name"] = Name;
        dictionary["BirthDate"] = BirthDate;
        dictionary["CodesInCSharp"] = CodesInCSharp;
        dictionary["NumberOfCopies"] = NumberOfCopies;
        dictionary["BackgroundNamedColor"] = BackgroundNamedColor.Name;
    }

    T GetDictionaryEntry<T>(IDictionary<string, object> dictionary, string key, T defaultValue = default(T))
    {
        return dictionary.ContainsKey(key) ? (T)dictionary[key] : defaultValue;
    }

    bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (Object.Equals(storage, value))
            return false;

        storage = value;
        OnPropertyChanged(propertyName);
        return true;
    }

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

각 응용 프로그램 설정 라는 메서드에서 Xamarin.Forms 속성 사전에 저장 되는 속성은 `SaveState` 생성자에서 해당 사전에서 로드 합니다. 클래스의 아래쪽으로 이동 하는 두 Viewmodel 간소화 하 고 오류가 발생할 가능성이 적으므로 있도록 메서드입니다. `OnPropertyChanged` 맨 아래에 메서드는이 호출 속성으로 설정 하는 선택적 매개 변수입니다. 이 문자열 속성의 이름을 지정할 때는 맞춤법 오류를 방지 합니다. 

`SetProperty` 클래스의 메서드는 훨씬 더 많은: 속성에는 필드와 저장 된 값으로 설정 되는 값과 비교 하 고 호출 `OnPropertyChanged` 두 값이 동일 하지 않습니다. 

`SampleSettingsViewModel` 배경색에 대 한 두 개의 속성을 정의 하는 클래스:는 `BackgroundNamedColor` 형식의 속성은 `NamedColor`, 하는 클래스에도 포함 되어는 **DataBindingDemos** 솔루션입니다. `BackgroundColor` 형식의 속성은 `Color`에서 얻은 및는 `Color` 의 속성은 `NamedColor` 개체입니다.

`NamedColor` 클래스.NET 리플렉션을 사용 하 여 xamarin.forms는 모든 정적 public 필드를 열거할 `Color` 구조, 정적에서 액세스할 수 있는 컬렉션의 이름으로 저장 하 고 `All` 속성:

```csharp
public class NamedColor : IEquatable<NamedColor>, IComparable<NamedColor>
{
    // Instance members
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    public bool Equals(NamedColor other)
    {
        return Name.Equals(other.Name);
    }

    public int CompareTo(NamedColor other)
    {
        return Name.CompareTo(other.Name);
    }

    // Static members
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof(Color))
            {
                // Convert the name to a friendly name.
                string name = fieldInfo.Name;
                stringBuilder.Clear();
                int index = 0;

                foreach (char ch in name)
                {
                    if (index != 0 && Char.IsUpper(ch))
                    {
                        stringBuilder.Append(' ');
                    }
                    stringBuilder.Append(ch);
                    index++;
                }

                // Instantiate a NamedColor object.
                Color color = (Color)fieldInfo.GetValue(null);

                NamedColor namedColor = new NamedColor
                {
                    Name = name,
                    FriendlyName = stringBuilder.ToString(),
                    Color = color,
                    RgbDisplay = String.Format("{0:X2}-{1:X2}-{2:X2}",
                                                (int)(255 * color.R),
                                                (int)(255 * color.G),
                                                (int)(255 * color.B))
                };

                // Add it to the collection.
                all.Add(namedColor);
            }
        }
        all.TrimExcess();
        all.Sort();
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }

    public static NamedColor Find(string name)
    {
        return ((List<NamedColor>)All).Find(nc => nc.Name == name);
    }

    public static string GetNearestColorName(Color color)
    {
        double shortestDistance = 1000;
        NamedColor closestColor = null;

        foreach (NamedColor namedColor in NamedColor.All)
        {
            double distance = Math.Sqrt(Math.Pow(color.R - namedColor.Color.R, 2) +
                                        Math.Pow(color.G - namedColor.Color.G, 2) +
                                        Math.Pow(color.B - namedColor.Color.B, 2));

            if (distance < shortestDistance)
            {
                shortestDistance = distance;
                closestColor = namedColor;
            }
        }
        return closestColor.Name;
    }
}
```

`App` 클래스에 **DataBindingDemos** 라는 속성을 정의 하는 프로젝트 `Settings` 형식의 `SampleSettingsViewModel`합니다. 이 속성은 초기화 때는 `App` 클래스가 인스턴스화되면 및 `SaveState` 메서드를 호출한 경우는 `OnSleep` 메서드를 호출 합니다.

```csharp
public partial class App : Application
{
    public App()
    {
        InitializeComponent();

        Settings = new SampleSettingsViewModel(Current.Properties);

        MainPage = new NavigationPage(new MainPage());
    }

    public SampleSettingsViewModel Settings { private set; get; }

    protected override void OnStart()
    {
        // Handle when your app starts
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Settings.SaveState(Current.Properties);
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
    }
}
```

응용 프로그램 수명 주기 방법에 대 한 자세한 내용은 문서 참조 [ **앱 수명 주기**](~/xamarin-forms/app-fundamentals/app-lifecycle.md)합니다.

다른 거의 모든 항목에서 처리 되는 **SampleSettingsPage.xaml** 파일입니다. `BindingContext` 사용 하 여 설정 되는 페이지의는 `Binding` 태그 확장: 바인딩 소스는 정적 `Application.Current` 인스턴스 속성을의 `App` 프로젝트의 클래스 및 `Path` 로 설정 되는 `Settings` 속성, 즉는 `SampleSettingsViewModel` 개체:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SampleSettingsPage"
             Title="Sample Settings"
             BindingContext="{Binding Source={x:Static Application.Current},
                                      Path=Settings}">

    <StackLayout BackgroundColor="{Binding BackgroundColor}"
                 Padding="10"
                 Spacing="10">

        <StackLayout Orientation="Horizontal">
            <Label Text="Name: "
                   VerticalOptions="Center" />

            <Entry Text="{Binding Name}"
                   Placeholder="your name"
                   HorizontalOptions="FillAndExpand"
                   VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Birth Date: "
                   VerticalOptions="Center" />

            <DatePicker Date="{Binding BirthDate}"
                        HorizontalOptions="FillAndExpand"
                        VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Do you code in C#? "
                   VerticalOptions="Center" />

            <Switch IsToggled="{Binding CodesInCSharp}"
                    VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Number of Copies: "
                   VerticalOptions="Center" />

            <Stepper Value="{Binding NumberOfCopies}"
                     VerticalOptions="Center" />

            <Label Text="{Binding NumberOfCopies}"
                   VerticalOptions="Center" />
        </StackLayout>

        <Label Text="Background Color:" />

        <ListView x:Name="colorListView"
                  ItemsSource="{x:Static local:NamedColor.All}"
                  SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
                  VerticalOptions="FillAndExpand"
                  RowHeight="40">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <BoxView Color="{Binding Color}"
                                     HeightRequest="32"
                                     WidthRequest="32"
                                     VerticalOptions="Center" />

                            <Label Text="{Binding FriendlyName}"
                                   FontSize="24"
                                   VerticalOptions="Center" />
                        </StackLayout>                        
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

페이지의 모든 자식 바인딩 컨텍스트를 상속합니다. 속성에이 페이지에 다른 바인딩 대부분 `SampleSettingsViewModel`합니다. `BackgroundColor` 속성은 설정 하는 데 사용 되는 `BackgroundColor` 의 속성은 `StackLayout`, 및 `Entry`, `DatePicker`, `Switch`, 및 `Stepper` 바인딩할 속성을 모든 기타 ViewModel 속성입니다.

`ItemsSource` 의 속성은 `ListView` static로 설정 되어 `NamedColor.All` 속성입니다. 그러면 채워집니다는 `ListView` 모든는 `NamedColor` 인스턴스. 각 항목에 대해는 `ListView`로 설정 되어 있는 항목에 대 한 바인딩 컨텍스트는 `NamedColor` 개체입니다. `BoxView` 및 `Label` 에 `ViewCell` 속성에 바인딩된 `NamedColor`합니다.

`SelectedItem` 속성의는 `ListView` 유형의 `NamedColor`에 바인딩되어는 `BackgroundNamedColor` 속성 `SampleSettingsViewModel`:

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

에 대 한 기본 바인딩 모드 `SelectedItem` 은 `OneWayToSource`, 선택한 항목에서 ViewModel 속성을 설정 하는 합니다. `TwoWay` 모드에서는 `SelectedItem` ViewModel에서 초기화 합니다. 

그러나 때는 `SelectedItem` 이러한 방식으로 설정 되어는 `ListView` 선택한 항목을 표시 하도록 자동으로 스크롤되지 않습니다. 코드 숨김 파일에는 작은 코드는 해야 합니다.

```csharp
public partial class SampleSettingsPage : ContentPage
{
    public SampleSettingsPage()
    {
        InitializeComponent();

        if (colorListView.SelectedItem != null)
        {
            colorListView.ScrollTo(colorListView.SelectedItem, 
                                   ScrollToPosition.MakeVisible, 
                                   false);
        }
    }
}
``` 

왼쪽에 iOS 스크린 샷을 처음 실행할 때 프로그램을 보여 줍니다. 생성자 `SampleSettingsViewModel` 초기화 배경색을 흰색으로 되며, 그에서 선택 된 항목의 `ListView`:

[![예제 설정](binding-mode-images/samplesettings-small.png "설정 샘플")](binding-mode-images/samplesettings-large.png#lightbox "예제 설정")

변경 된 설정 표시 하는 다른 두 가지 스크린샷입니다. 이 페이지를 시험해 프로그램 절전 모드로 전환 하거나 장치 또는 실행 중인 에뮬레이터에서 종료를 전환 해야 합니다. Visual Studio 디버거에서 프로그램 종료를 발생 하지 것입니다는 `OnSleep` 의 재정의 `App` 클래스를 호출할 수 있습니다.

다음 문서에서 지정 하는 방법을 확인할 [ **문자열 서식 지정** ](string-formatting.md) 에 설정 된 데이터 바인딩는 `Text` 속성 `Label`합니다.


## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 책에서 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
