---
layout: post
key: 20171121
modify_date: 2017-12-04
tags: [C#, MVC, chart.js]
title: MVC convert HTML to PDF
---

Using [WkHtmlToXSharp](https://github.com/pruiz/WkHtmlToXSharp){:target="_blank"} to convert HTML into PDF, plus a simple download function using **System.IO.MemoryStream**

<!--more-->

Note:

1. the resource files (js, css etc.) must come with absolute paths. 

2. Animations if any, will also have to be disabled since there is no delay in redendering available in WkHtmlToXSharp.

3. In my case the PDF contains charts generated using [Chart.js](http://www.chartjs.org/){:target="_blank"}, **animation**, **responsiveAnimationDuration** and **responsive** must be set to false.

{% highlight JavaScript %}
options: {
          animation: false,
          hover: {
              animationDuration: 0
          },
          responsiveAnimationDuration: 0,
          responsive: false
         }
{% endhighlight %}

## 1. Invoke the procedure
A simple method serves as main method to kickoff the whole procedure. The absolute root path is extracted and used to replace relative paths.

{% highlight C# %}

   public void DownloadReport()
   {
       string userId = User.Identity.GetUserId();

       string htmlRendered = ConvertViewToString("_FeedbackPDF", GetFeedbackReportViewModel(userId));

       // convert resource path into absolute
       string path = new Uri(Request.Url, Url.Content("~/")).ToString();

       htmlRendered = htmlRendered.Replace("=\"/", "=\""+path);

       byte[] bytes = ConvertPDF(htmlRendered);

       MemoryStream workStream = new MemoryStream();
       workStream.Write(bytes, 0, bytes.Length);
       workStream.Position = 0;

       Response.Buffer = true;
       Response.AddHeader("Content-Disposition", "attachment; filename= " + Server.HtmlEncode("PDF.pdf"));
       Response.ContentType = "APPLICATION/pdf";
       Response.BinaryWrite(bytes);
   }

{% endhighlight %}

## 2. Convert a View To HTML String

HTML string is generated directly from a view.
{% highlight C# %}

   private string ConvertViewToString(string viewName, object model)
   {
       ViewData.Model = model;
       using (StringWriter stringWriter = new StringWriter())
       {
           ViewEngineResult viewEngineResult = ViewEngines.Engines.FindPartialView(ControllerContext, viewName);
           ViewContext viewContext = new ViewContext(this.ControllerContext, viewEngineResult.View, ViewData, new TempDataDictionary(), stringWriter);
           viewEngineResult.View.Render(viewContext, stringWriter);
           return stringWriter.ToString();
       }
   }
{% endhighlight %}


## 3. Convert HTML string into byte array

{% highlight C# %}
   private byte[] ConvertPDF(string htmlRendered)
   {
       TryRegisterLibraryBundles();

       IHtmlToPdfConverter converter = new MultiplexingConverter();
       // Display page numbers in footer
       converter.ObjectSettings.Footer.Right = "Page [page] of [toPage]";
       byte[] bytes;
       try
       {
           bytes = converter.Convert(htmlRendered);
       }
       catch (Exception)
       {
           throw;
       }

       return bytes;
   }

{% endhighlight %}

## 4. Register WkHtmlToXLibraries

This registers WkHtmlToXLibraries for the corresponding platform, in this case, Win64.

{% highlight C# %}

   private void TryRegisterLibraryBundles()
   {
       var ignore = Environment.GetEnvironmentVariable("WKHTMLTOXSHARP_NOBUNDLES");

       if (ignore == null || ignore.ToLower() != "true")
       {
           //WkHtmlToXLibrariesManager.Register(new Linux32NativeBundle());
           //WkHtmlToXLibrariesManager.Register(new Linux64NativeBundle());
           //WkHtmlToXLibrariesManager.Register(new Win32NativeBundle());
           WkHtmlToXLibrariesManager.Register(new Win64NativeBundle());
       }
   }

{% endhighlight %}
