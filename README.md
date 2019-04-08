## Awesome Timeline in Xamarin.Forms

![](Timeline/screenshots/tweet.png)

### Lioncoding article Link

[Awesome Timeline in Xamarin.Forms](https://lioncoding.com/)

### Create your project(I'm using VS2019)

![](Timeline/screenshots/select_type.PNG)



![](Timeline/screenshots/project_location.PNG)

![](Timeline/screenshots/project_model.PNG)

### Add the following NuGet packages to your solution

- [Xamarin.FFImageLoading.Forms 2.4.4.859](https://www.nuget.org/packages/)
- [Xamarin.FFImageLoading.Transformations 2.4.4.859](https://www.nuget.org/packages/)
- [Xamarin.Forms 3.4.0.1009999](https://www.nuget.org/packages/)

### Initialization

##### Android project

- MainActivity.cs

```csharp
protected override void OnCreate(Bundle savedInstanceState)
{
	TabLayoutResource = Resource.Layout.Tabbar;
	ToolbarResource = Resource.Layout.Toolbar;

	base.OnCreate(savedInstanceState);
    // Init FFImageLoading plugin
	CachedImageRenderer.Init(false);
	global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
	LoadApplication(new App());
}
```



### Our Event Model

```csharp
namespace Timeline.Models
{
    public class Event
    {
        public string AuthorImage { get; set; }
        public string Title { get; set; }
        public string Detail { get; set; }
        public string DateToString { get; set; }
    }
}
```

### Service which provide event list

```csharp
using System.Collections.Generic;
using Timeline.Models;

namespace Timeline.Services
{
    public class EventService
    {
        public static List<Event> GetAllEvents()
        {
            return new List<Event>
            {
                new Event
                {
                    AuthorImage ="me.jpg",
                    Title = "Laurent has a new photo",
                    Detail = "Laurent uploaded a more recently pic from him.",
                    DateToString = "APRIL, 08 2019"
                },
                new Event
                {
                    AuthorImage ="ispace.jpg",
                    Title = "Ispace Corporation liked your Tweet",
                    Detail = "Ispace Corporation liked your last Tweet: Ready to share with ...",
                    DateToString = "APRIL, 08 2019"
                },
                new Event
                {
                    AuthorImage ="lfrii.jpg",
                    Title = "L-frii Retweeted",
                    Detail = "L-frii Retweeted your Tweet.",
                    DateToString = "APRIL, 08 2019"
                },
                 new Event
                {
                    AuthorImage ="mvp.jpg",
                    Title = "MVP Award Program Retweeted your Tweet.",
                    Detail = "MVP Award Program Retweeted your Tweet: MVP Gbobal Submmit...",
                    DateToString = "APRIL, 08 2019"
                },
                 new Event
                {
                    AuthorImage ="sani.jpg",
                    Title = "You have a new follower!",
                    Detail = "Sani Koffi is now following you.",
                    DateToString = "APRIL, 08 2019"
                },
                new Event
                {
                    AuthorImage ="leomaris.jpg",
                    Title = "Leomaris Rayes has a new photo",
                    Detail = "Leomaris Rayes uploaded a more recently pic from her.",
                    DateToString = "APRIL, 08 2019"
                },
                new Event
                {
                    AuthorImage ="houssem.jpg",
                    Title = "Houssem Dellai liked your reply.",
                    Detail = "Houssem Dellai liked your reply: Congratulations !!!",
                    DateToString = "MARCH, 10 2019"
                },
                new Event
                {
                    AuthorImage ="nasa.jpg",
                    Title = "NASA started a broadcast",
                    Detail = "NASA started a new broadcast.",
                    DateToString = "MARCH, 14 2019"
                },
                new Event
                {
                    AuthorImage ="googleanalytics.jpg",
                    Title = "Google Ananytics liked your reply.",
                    Detail = "Google Ananytics and 5 others liked your reply.",
                    DateToString = "JANUARY, 08 2019"
                }               
            };
        }
    }
}

```

### ViewModel source code

```csharp
using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Text;
using Timeline.Models;

namespace Timeline.ViewModels
{
    public class TimelineViewModel : BaseViewModel
    {
        public TimelineViewModel()
        {
            Title = "Timeline";
            TimelineEvents = new ObservableCollection<Event>
                (Services.EventService.GetAllEvents());
        }


        private ObservableCollection<Event> _timelineEvents;
        public ObservableCollection<Event> TimelineEvents
        {
            get { return _timelineEvents; }
            set { SetProperty(ref _timelineEvents, value); }
        }
    }
}
```

### UI

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage
    x:Class="Timeline.Views.TimelineView"
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:ff="clr-namespace:FFImageLoading.Forms;assembly=FFImageLoading.Forms"
    xmlns:ffTransformations="clr-namespace:FFImageLoading.Transformations;assembly=FFImageLoading.Transformations"
    xmlns:viewModel="clr-namespace:Timeline.ViewModels"
    Title="{Binding Title}"
    BackgroundImage="bg.jpg">

    <ContentPage.BindingContext>
        <viewModel:TimelineViewModel />
    </ContentPage.BindingContext>

    <ContentPage.Content>
        <StackLayout>
            <ListView
                CachingStrategy="RecycleElement"
                HasUnevenRows="False"
                ItemsSource="{Binding TimelineEvents}"
                RowHeight="107"
                SelectionMode="None"
                SeparatorColor="Gray"
                SeparatorVisibility="None">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <ViewCell>
                            <StackLayout
                                Margin="20,0,0,0"
                                Orientation="Horizontal"
                                VerticalOptions="Center">
                                <StackLayout
                                    x:Name="firstStackLayout"
                                    Margin="0,0,0,-6"
                                    HorizontalOptions="Center"
                                    Orientation="Vertical"
                                    VerticalOptions="Center">
                                    <BoxView
                                        Grid.Row="0"
                                        Grid.Column="0"
                                        Margin="0,0,0,-6"
                                        HeightRequest="30"
                                        HorizontalOptions="Center"
                                        WidthRequest="3"
                                        Color="Accent" />
                                    <ff:CachedImage
                                        Grid.Row="1"
                                        Grid.Column="0"
                                        Margin="0,0,0,0"
                                        HeightRequest="55"
                                        Source="{Binding AuthorImage}"
                                        WidthRequest="55">
                                        <ff:CachedImage.Transformations>
                                            <ffTransformations:RoundedTransformation
                                                BorderHexColor="#FF4081"
                                                BorderSize="20"
                                                Radius="240" />
                                        </ff:CachedImage.Transformations>
                                    </ff:CachedImage>
                                    <BoxView
                                        Grid.Row="2"
                                        Grid.Column="0"
                                        Margin="0,-6,0,0"
                                        HeightRequest="30"
                                        HorizontalOptions="Center"
                                        WidthRequest="3"
                                        Color="Accent" />
                                </StackLayout>
                                <StackLayout
                                    Margin="5,0,0,0"
                                    HorizontalOptions="FillAndExpand"
                                    Orientation="Horizontal"
                                    VerticalOptions="Center">
                                    <StackLayout
                                        Margin="0,0,5,0"
                                        HorizontalOptions="Start"
                                        Orientation="Vertical"
                                        VerticalOptions="Center">
                                        <Label
                                            FontAttributes="Bold"
                                            FontSize="15"
                                            HorizontalOptions="Start"
                                            Text="{Binding Title}"
                                            TextColor="Accent"
                                            XAlign="Start" />
                                        <StackLayout
                                            Margin="0,0,5,0"
                                            Orientation="Horizontal"
                                            VerticalOptions="EndAndExpand">
                                            <Label
                                                FontSize="14"
                                                Text="{Binding Detail}"
                                                TextColor="#4e5156" />
                                        </StackLayout>
                                        <StackLayout Orientation="Horizontal" VerticalOptions="EndAndExpand">
                                            <Label
                                                FontAttributes="Bold"
                                                FontSize="12"
                                                Text="{Binding DateToString}"
                                                TextColor="#3b0999" />
                                        </StackLayout>
                                    </StackLayout>
                                </StackLayout>
                            </StackLayout>
                        </ViewCell>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```
