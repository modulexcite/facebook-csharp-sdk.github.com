---
layout: default
title: FriendPicker Control
platform: phone
---

In this document:

* [Overview](#1)
* [Using the FriendPicker Control](#2)
* [See Also](#3)

---

## Overview

The [FriendPicker](/docs/reference/client/Client.Controls.FriendPicker.html) control displays a user interface that you can use to pick friends in your Facebook profile. 

---

## Using the FriendPicker Control

In this tutorial, you will add a [FriendPicker](/docs/reference/client/Client.Controls.FriendPicker.html) control to an application that adds the capability for selecting friends in your Facebook profile. This tutorial builds on top of the [LoginButton control tutorial](/docs/phone/controls/login-ui-control/), which you need to complete before proceeding; in particular, you should have already added the **Facebook.Client NuGet** to your project as well as inserted and configured a [LoginButton](/docs/reference/client/Client.Controls.LoginButton.html) in your page.

1.  Open the **MainPage.xaml** page of the application that you created for the [LoginButton control tutorial](/docs/phone/controls/login-ui-control/). Alternatively, you can start with the result of the [ProfilePicture control tutorial](/docs/phone/controls/profilepicture-ui-control/), which also adds a [ProfilePicture](/docs/reference/client/Client.Controls.ProfilePicture.html) control to the page.

1.  In the content area in the center row, locate the **Grid** element and replace any nested content with the following row definitions that create a layout with two rows, with the top row reserved for the [FriendPicker](/docs/reference/client/Client.Controls.FriendPicker.html) control and the bottom row for the details view.

        <!--ContentPanel - place additional content here-->
        <Grid x:Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0" Visibility="Collapsed">
            <Grid.RowDefinitions>
                <RowDefinition />
                <RowDefinition MaxHeight="170"/>
            </Grid.RowDefinitions>
        </Grid>

1.  Next, inside this content panel, insert a [FriendPicker](/docs/reference/client/Client.Controls.FriendPicker.html) control.

        <facebookControls:FriendPicker 
            x:Name="friendPicker" />

            
> Note: The FriendsPicker control relies on the AccessToken property to be set to fetch the list of your friends. If you use the [LoginButton](/docs/reference/client/Client.Controls.LoginButton.html), you don't need to do any further work. As soon as the user logs in, the FriendPicker will get notified and populate itself. 


1.  Build and run the application and click **Log In** to retrieve a list of friends in your Facebook profile.  

    ![image](images/image23.png)

1.  Scroll through the list and notice how friends are categorized according to the first letter of their name.  If the list is extensive, finding a friend can be difficult. To make this task easier, the [FriendPicker](/docs/reference/client/Client.Controls.FriendPicker.html) control includes a jump list feature that allows you to quickly navigate through the list. To see it in action, click the heading for a category to zoom out and display the jump list.
 
    ![image](images/image24.png)

1.  The jump list shows selectors for every letter in the alphabet. Selectors are highlighted to indicate that the group contains at least one member. Click any of the highlighted selectors to zoom in and navigate to that group.

1.  Click the entry for a friend in the list to select it. By default, multiple friends may be selected and clicking additional friends adds them to the current selection.

1.  Shut down the running program. You will now update the page to explore additional features of the [FriendPicker](/docs/reference/client/Client.Controls.FriendPicker.html) control by including a details view that shows the current selection and allows you to control some of its properties.

1.  To define the details view, insert another **Grid** element nested inside the content area, placing it immediately below the **FriendPicker**. The row and column definitions create the layout of the details section. 

        <!-- details section -->
        <Grid Grid.Row="1" Margin="2">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
        </Grid>

1.  By default, the [FriendPicker](/docs/reference/client/Client.Controls.FriendPicker.html) control displays a profile picture alongside the name of each friend shown in the list. This behavior is controlled by the **DisplayProfilePictures** property, which determines whether a picture is shown. To see how this property works, insert the following XAML markup inside the details section to display a check box that enables or disables the display of pictures.

        <!-- display pictures -->
        <StackPanel Orientation="Horizontal" VerticalAlignment="Top">
            <CheckBox 
                Margin="0,-10,-10,-10"
                x:Name="displayPictures" 
                VerticalAlignment="Top"
                IsChecked="{Binding DisplayProfilePictures, ElementName=friendPicker, Mode=TwoWay}" />
            <TextBlock
                Margin="0"
                FontSize="16"
                VerticalAlignment="Center"
                HorizontalAlignment="Right"
                Style="{StaticResource PhoneTextNormalStyle}" 
                Text="Display pictures" />
        </StackPanel>

1.  Build and run the application. Clear the check box to see the list change to display only the name of your friends.
 
    ![image](images/image25.png)

1.  The order in which names are displayed, last name first or first name first, is determined by the **DisplayOrder** property. By default, names on the list show the last name first. To change the display order, insert the following XAML markup inside the details section adding radio buttons that control the display order.

        <!-- display order -->
        <StackPanel Grid.Row="1">
            <RadioButton 
                x:Name="DisplayFirstNameFirst"
                Margin="0,-15"
                FontSize="16"
                Content="First, Last name" 
                GroupName="DisplayOrder" 
                Checked="OnDisplayOrderSelected" />
            <RadioButton 
                x:Name="DisplayLastNameFirst"
                Margin="0,-15"
                FontSize="16"
                Content="Last, First name" 
                GroupName="DisplayOrder" 
                IsChecked="True"
                Checked="OnDisplayOrderSelected" />
        </StackPanel>

1.  To respond to changes in the radio buttons and set the **DisplayOrder** property of the [FriendPicker](/docs/reference/client/Client.Controls.FriendPicker.html) control, add the following event handler to the page’s code-behind file. Note that you could also bind the radio buttons to the property by a creating a value converter instead of handling the event. 

        private void OnDisplayOrderSelected(object sender, RoutedEventArgs e)
        {
            if (this.friendPicker != null)
            {
                var choice = sender as RadioButton;
                this.friendPicker.DisplayOrder =
                    (Facebook.Client.Controls.FriendPickerDisplayOrder)Enum.Parse(typeof(Facebook.Client.Controls.FriendPickerDisplayOrder), choice.Name);
            }
        }
    
1.  Build and run the application. Click the radio buttons to see the display order of the names change to match your selection.
 
    ![image](images/image26.png)

1.  The [FriendPicker](/docs/reference/client/Client.Controls.FriendPicker.html) control offers several modes of selection including _None_, _Single_, _Multiple_, and _Extended_, which you specify using the **SelectionMode** property. The current selection is available through the **SelectedItems** property. To show the current selection, insert the following XAML markup inside the details section adding a **ListBox** bound to the **SelectedItems** property of the [FriendPicker](/docs/reference/client/Client.Controls.FriendPicker.html).

        <!-- current selection -->
        <TextBlock 
            Grid.Column="1"
            FontSize="16"
            VerticalAlignment="Center" 
            Text="Selected friends" />
        <ListBox
            Grid.Row="1"
            Grid.Column="1"                
            VerticalAlignment="Top"
            ItemsSource="{Binding SelectedItems, ElementName=friendPicker}">
            <ListBox.ItemTemplate>
                <DataTemplate>
                    <StackPanel>
                        <TextBlock Text="{Binding Name}" FontSize="16" />
                        <TextBlock Text="{Binding Location.City}" FontSize="12" />
                    </StackPanel>
                </DataTemplate>
            </ListBox.ItemTemplate>
        </ListBox>

1.  Build and run the application. Select friends in the [FriendPicker](/docs/reference/client/Client.Controls.FriendPicker.html) and see how their names are added to the list on the bottom right. Similarly, clearing any selected item in the [FriendPicker](/docs/reference/client/Client.Controls.FriendPicker.html) removes the name from this list.
 
    ![image](images/image27.png)

1.  If you examine the item template for the **ListView** that displays the current selection, you will see that it not only binds the **Name** of the selected person, but also its **Location.City**. Despite this, when you ran the application before, you may have noticed that the list only showed the names of those people you selected. The reason is that the location data is not included by default among the information retrieved by the control. To include it, you need to update the **DisplayFields** property to add the _location_ field, as shown below. Notice that, by default, the control already retrieves other properties such as _id_, _first_, _middle_, and _last name_, and _picture_.
 
    ![image](images/image28.png) 

    ![image](images/image29.png)

1.  One additional requirement needs to be fulfilled before full access to location data is possible. If you run the application now, you may notice that the list on the right does not display the location for some of your friends. Because location data is optional, it is possible that your friend’s profile does not include this information.  Another reason for the missing location data, is that you friend has restricted public access to their location. Nevertheless, and assuming your friend has already granted access to the location to his or her friends, you can request permission to retrieve this piece of information when you log in. To do this, locate the **LoginButton** on the page, and add _friends_location_ to the list of **Permissions**. This will ensure that the access token negotiated by the login control has permissions to retrieve your friends’ location.
 
    ![image](images/image30.png) 

    ![image](images/image31.png)

1.  The sample application for this tutorial is ready at this point. The complete XAML for the page is shown below. Compare it with your work to ensure that you have followed the steps correctly.

        <phone:PhoneApplicationPage
            x:Class="FacebookControls_WP8.MainPage"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:phone="clr-namespace:Microsoft.Phone.Controls;assembly=Microsoft.Phone"
            xmlns:shell="clr-namespace:Microsoft.Phone.Shell;assembly=Microsoft.Phone"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:facebookControls="clr-namespace:Facebook.Client.Controls;assembly=Facebook.Client"
            mc:Ignorable="d"
            FontFamily="{StaticResource PhoneFontFamilyNormal}"
            FontSize="{StaticResource PhoneFontSizeNormal}"
            Foreground="{StaticResource PhoneForegroundBrush}"
            SupportedOrientations="Portrait" Orientation="Portrait"
            shell:SystemTray.IsVisible="True">

            <!--LayoutRoot is the root grid where all page content is placed-->
            <Grid x:Name="LayoutRoot" Background="Transparent">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="*"/>
                    <RowDefinition Height="Auto"/>
                </Grid.RowDefinitions>

                <!--TitlePanel contains the name of the application and page title-->
                <StackPanel Grid.Row="0" Margin="2">
                    <TextBlock Text="MY APPLICATION" Style="{StaticResource PhoneTextNormalStyle}"/>
                </StackPanel>

                <!--ContentPanel - place additional content here-->
                <Grid x:Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0" Visibility="Collapsed">
                    <Grid.RowDefinitions>
                        <RowDefinition />
                        <RowDefinition MaxHeight="170"/>
                    </Grid.RowDefinitions>

                    <facebookControls:FriendPicker 
                        x:Name="friendPicker"
                        DisplayFields="id,name,first_name,middle_name,last_name,picture,location"/>

                    <!-- details section -->
                    <Grid Grid.Row="1" Margin="2">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto"/>
                            <RowDefinition />
                        </Grid.RowDefinitions>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition />
                            <ColumnDefinition />
                        </Grid.ColumnDefinitions>

                        <!-- display pictures -->
                        <StackPanel Orientation="Horizontal" VerticalAlignment="Top">
                            <CheckBox 
                                Margin="0,-10,-10,-10"
                                x:Name="displayPictures" 
                                VerticalAlignment="Top"
                                IsChecked="{Binding DisplayProfilePictures, ElementName=friendPicker, Mode=TwoWay}" />
                            <TextBlock
                                Margin="0"
                                FontSize="16"
                                VerticalAlignment="Center"
                                HorizontalAlignment="Right"
                                Style="{StaticResource PhoneTextNormalStyle}" 
                                Text="Display pictures" />
                        </StackPanel>

                        <!-- display order -->
                        <StackPanel Grid.Row="1">
                            <RadioButton 
                                x:Name="DisplayFirstNameFirst"
                                Margin="0,-15"
                                FontSize="16"
                                Content="First, Last name" 
                                GroupName="DisplayOrder" 
                                Checked="OnDisplayOrderSelected" />
                            <RadioButton 
                                x:Name="DisplayLastNameFirst"
                                Margin="0,-15"
                                FontSize="16"
                                Content="Last, First name" 
                                GroupName="DisplayOrder" 
                                IsChecked="True"
                                Checked="OnDisplayOrderSelected" />
                        </StackPanel>

                        <!-- current selection -->
                        <TextBlock 
                            Grid.Column="1"
                            FontSize="16"
                            VerticalAlignment="Center" 
                            Text="Selected friends" />
                        <ListBox
                            Grid.Row="1"
                            Grid.Column="1"                
                            VerticalAlignment="Top"
                            ItemsSource="{Binding SelectedItems, ElementName=friendPicker}">
                            <ListBox.ItemTemplate>
                                <DataTemplate>
                                    <StackPanel>
                                        <TextBlock Text="{Binding Name}" FontSize="16" />
                                        <TextBlock Text="{Binding Location.City}" FontSize="12" />
                                    </StackPanel>
                                </DataTemplate>
                            </ListBox.ItemTemplate>
                        </ListBox>
                    </Grid>
                </Grid>

                <!--user information-->
                <StackPanel 
                    Grid.Row="2" 
                    Orientation="Horizontal" 
                    HorizontalAlignment="Left" 
                    Margin="5">
                    <facebookControls:ProfilePicture 
                        x:Name="profilePicture"
                        Width="60"
                        Height="60" 
                        CropMode="Original" 
                        ProfileId="{Binding CurrentSession.FacebookId, ElementName=loginButton}" />
                    <TextBlock 
                        Margin="10,0,0,0"
                        HorizontalAlignment="Center"
                        VerticalAlignment="Center" 
                        Text="{Binding CurrentUser.Name, ElementName=loginButton}" />
                </StackPanel>

                <!-- login control -->
                <facebookControls:LoginButton 
                    x:Name="loginButton" 
                    Grid.Row="2" 
                    Margin="5"
                    HorizontalAlignment="Right" 
                    ApplicationId="427365490674294" 
                    Permissions="friends_location"
                    SessionStateChanged="OnSessionStateChanged" />
            </Grid>

        </phone:PhoneApplicationPage>
    
1.  Build and run the application. When you log in, you will be prompted for permission to access your friends’ current location. Click **Okay** to grant access.
 
    ![image](images/image32.png)

1.  Select one or more friends in the [FriendPicker](/docs/reference/client/Client.Controls.FriendPicker.html) and notice how their current location is now shown by the list on the right.

    ![image](images/image33.png)

---

## See Also
