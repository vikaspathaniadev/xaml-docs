---
title: Loading Data from WCF Services
page_title: Loading Data from WCF Services
description: Loading Data from WCF Services
slug: gridview-loading-data-from-wcf-services
tags: loading,data,from,wcf,services
published: True
position: 6
---

# Loading Data from WCF Services

The purpose of this tutorial is to show you how to populate __RadGridView__ with data from a __WCF Service__ in two ways:

* A straight-forward way - [plain method calls](#Plain_Method_Calls). 

* [Using MVVM approach](#Using_MVVM_Approach)

>This tutorial will use the __Northwind__ database, which can be downloaded from [here](http://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46A0-8DA2-EEBC53A68034&displaylang=en).

Before proceeding further with this tutorial you need to create a new application and add a __RadGridView__ declaration in your XAML: 

#### __XAML__

{{region xaml-gridview-loading-data-from-wcf-services_0}}
	<telerik:RadGridView />
{{endregion}}

## Plain Method Calls

* Add a reference to your WCF Service. 

* Switch to the code-behind and create a new instance of your WCF Service client.

#### __C#__

{{region cs-gridview-loading-data-from-wcf-services_1}}
	WcfServiceClient serviceClient = new WcfServiceClient();
{{endregion}}

#### __VB.NET__

{{region vb-gridview-loading-data-from-wcf-services_2}}
	Private serviceClient As New WcfServiceClient()
{{endregion}}

>tipFor more information about how to add a reference to a WCF Service and how to create a new instance of a WCF Service client, take a look at the {% if site.site_name == 'Silverlight' %}[Consuming WCF Service](http://www.telerik.com/help/silverlight/consuming-data-wcf-service.html){% endif %}{% if site.site_name == 'WPF' %}[Consuming WCF Service](http://www.telerik.com/help/wpf/consuming-data-wcf-service.html){% endif %} topic.

* The gridview control will be populated with all __Customers__ from the __Northwind__ database. Add the following code which will make the initial load of the objects.

#### __C#__

{{region cs-gridview-loading-data-from-wcf-services_3}}
	private void BeginRequest()
	{
	    serviceClient.LoadCustomersCompleted += new EventHandler<LoadCustomersCompletedEventArgs>(serviceClient_LoadCustomersCompleted);
	    serviceClient.LoadCustomersAsync();
	}
	
	private void serviceClient_LoadCustomersCompleted(object sender, LoadCustomersCompletedEventArgs e)
	{
	    var customers = e.Result;
	    this.radGridView.ItemsSource = customers;
	}
{{endregion}}

#### __VB.NET__

{{region vb-gridview-loading-data-from-wcf-services_4}}
	Private Sub BeginRequest()
	    AddHandler serviceClient.LoadCustomersCompleted, AddressOf serviceClient_LoadCustomersCompleted
	    serviceClient.LoadCustomersAsync()
	End Sub
	
	Private Sub serviceClient_LoadCustomersCompleted(ByVal sender As Object, ByVal e As LoadCustomersCompletedEventArgs)
	    Dim customers = e.Result
	    Me.radGridView.ItemsSource = customers
	End Sub
{{endregion}}

#### __C#__

{{region cs-gridview-loading-data-from-wcf-services_5}}
	this.radGridView.ItemsSource = serviceClient.LoadCustomers();
{{endregion}}

#### __VB.NET__

{{region vb-gridview-loading-data-from-wcf-services_6}}
	Me.radGridView.ItemsSource = serviceClient.LoadCustomers()
{{endregion}}

Run your demo, the result can be seen on the next image:

![](images/RadGridView_PopulatingWithDataLoadFromWcf_010.PNG)

## Using MVVM Approach

This section will show you how to populate your __RadGridView__ control in a MVVM manner. The __RadGridView__ will be bound to a data source object, that has a property __Customers__. When the control is loaded all customers from the Customers table in the Northwind database are loaded asynchronously.

* Create a new class named __NorthwindDataSource__. 

#### __C#__

{{region cs-gridview-loading-data-from-wcf-services_7}}
	public class NorthwindDataSource
	{
	}
{{endregion}}

#### __VB.NET__

{{region vb-gridview-loading-data-from-wcf-services_8}}
	Public Class NorthwindDataSource
	End Class
{{endregion}}

* Add a reference to your WCF Service 

* In the __NorthwindDataSource__ class add a reference to an __ObservableCollection of Customers__. 

* In the __NorthwindDataSource__ class add a reference to your WCF Service client: 

#### __C#__

{{region cs-gridview-loading-data-from-wcf-services_9}}
	public class NorthwindDataSource
	{
	    private SampleWcfServiceClient serviceClient;
	    public NorthwindDataSource()
	    {
	        serviceClient = new SampleWcfServiceClient();
	        this.Customers = new ObservableCollection<Customer>();
	    }
	    public ObservableCollection<Customer> Customers
	    {
	        get;
	        set;
	    }
	}
{{endregion}}

#### __VB.NET__

{{region vb-gridview-loading-data-from-wcf-services_10}}
	Public Class NorthwindDataSource
	    Private serviceClient As SampleWcfServiceClient
	    Public Sub New()
	        serviceClient = New SampleWcfServiceClient()
	        Me.Customers = New ObservableCollection(Of Customer)()
	    End Sub
	    Public Property Customers() As ObservableCollection(Of Customer)
	End Class
{{endregion}}

>tipFor more information about how to add a reference to a WCF Service and how to create a new instance of a WCF Service client, take a look at the 
{% if site.site_name == 'Silverlight' %}[Consuming WCF Service](http://www.telerik.com/help/silverlight/consuming-data-wcf-service.html){% endif %}{% if site.site_name == 'WPF' %}[Consuming WCF Service](http://www.telerik.com/help/wpf/consuming-data-wcf-service.html){% endif %} topic.

* Add the following code in the constructor of the __NorthwindDataSource__. It will make the initial load of all __Customers__ from the database: 

#### __C#__

{{region cs-gridview-loading-data-from-wcf-services_11}}
	serviceClient.LoadCustomersCompleted += new EventHandler<LoadCustomersCompletedEventArgs>(serviceClient_LoadCustomersCompleted);
	serviceClient.LoadCustomersAsync();
{{endregion}}

#### __C#__

{{region cs-gridview-loading-data-from-wcf-services_12}}
	foreach (Customer c in serviceClient.LoadCustomers())
	{
	    this.Customers.Add(c);
	}
{{endregion}}

#### __VB.NET__

{{region vb-gridview-loading-data-from-wcf-services_13}}
	AddHandler serviceClient.LoadCustomersCompleted, AddressOf serviceClient_LoadCustomersCompleted
	serviceClient.LoadCustomersAsync()
{{endregion}}

#### __VB.NET__

{{region vb-gridview-loading-data-from-wcf-services_14}}
	For Each c As Customer In serviceClient.LoadCustomers()
	    Me.Customers.Add(c)
	Next c
{{endregion}}

{% if site.site_name == 'Silverlight' %}

And here is the code handling the __LoadCustomersCompleted__ event:{% endif %}

#### __C#__

{{region cs-gridview-loading-data-from-wcf-services_15}}
	if (e.Error == null && e.Result != null)
	{
	    foreach (Customer c in e.Result)
	    {
	        this.Customers.Add(c);
	    }
	}
{{endregion}}

#### __VB.NET__

{{region vb-gridview-loading-data-from-wcf-services_16}}
	If e.Error Is Nothing AndAlso e.Result IsNot Nothing Then
	    For Each c As Customer In e.Result
	        Me.Customers.Add(c)
	    Next c
	End If
{{endregion}}

* Declare the __NorthwindDataSource__ object as a resource in your application. 

#### __XAML__

{{region xaml-gridview-loading-data-from-wcf-services_17}}
	<UserControl.Resources>
	    <local:NorthwindDataSource x:Key="DataSource"/>
	</UserControl.Resources>
{{endregion}}

* Update your __RadGridView__ declaration - set the __ItemsSource__ property. 

#### __XAML__

{{region xaml-gridview-loading-data-from-wcf-services_18}}
	<telerik:RadGridView ItemsSource="{Binding Source={StaticResource DataSource}, Path=Customers}"/>
{{endregion}}

Run your demo, the result can be seen on the next picture: 

![](images/RadGridView_PopulatingWithDataLoadFromWcf_010.PNG)

>tipIf you need to define the columns manually take a look at the [Defining Columns]({%slug gridview-columns-defining-columns%}) topic.

## See Also

 * [Using in-memory Data]({%slug gridview-in-memory-date%})

 * [Loading Data from XML]({%slug gridview-loading-data-from-xml%})
 
 * [Loading Data from ADO.NET Services]({%slug gridview-loading-data-from-adonet-services%})
 
{% if site.site_name == 'Silverlight' %}
 * [Loading Data from RIA Services]({%slug gridview-loading-data-from-ria-services%})

 * [DomainDataSource Support]({%slug gridview-domaindatasource-support%})

 * [Loading Data from Web Services]({%slug gridview-loading-data-from-web-services%})
{% endif %}