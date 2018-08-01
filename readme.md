### This sample shows how to scale the width of an image via a coefficient using a ValueConverter.

The value converter looks like this:

	public class CoefConverter : IValueConverter
	{
		public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
		{
			double coef = 1.0;

			if (parameter is string)
				coef = double.Parse(parameter as string);

			return (double)value * coef;
		}

		public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
		{
			throw new NotImplementedException();
		}
	}

It can then be used in xaml to scale the width of the image. If the Aspect of the image is set to AspectFit, then the height will be automatically scaled as well:

    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:local="clr-namespace:ScaleImage"
                x:Class="ScaleImage.MainPage">

        <ContentPage.Resources>
            <ResourceDictionary>
                <local:CoefConverter x:Key="cc" />
            </ResourceDictionary>
        </ContentPage.Resources>

        <StackLayout x:Name="theStack">
            
            <Image Source="mountain.jpg" Aspect="AspectFit" HeightRequest="{Binding Source={x:Reference theStack}, Path=Width, Converter={StaticResource cc}, ConverterParameter=0.25}"  />
            <Label Text="This is some text" />

        </StackLayout>

    </ContentPage>