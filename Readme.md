## Emils-Image_of_a_scraped_object

### Idea:

- Scrapes a website (https://www.edoganci.dk) (my old website).
- Finds something specific (In this case, the image on the front page of the website).
- Replaces the web server title with the link to the image.
- The web server's javascript places the image in the background of a canvas.
- The ball moves in the canvas and shows a section of the image.

### Description:
I have both made a PC version that runs the web server and performs exactly the same task and in addition I have also implemented it for the Arduino IDE and performed the task with the esp8266 device that was provided.

At the bottom of the page there are snippets of the most important functions of the code.

### Demonstration:
- Result:

![](https://github.com/digitalInteraktion2019/IOTresources/blob/master/Emils-ImageBalling/Ressourcer/Demo.gif)


### Utilities - PC Version:
- Visual Studio 2019
- Curl (https://curl.haxx.se/)
- Frontend (https://freefrontend.com/)
	- Html/Css/Javascript
- Built-in WinHttpClient library (WebServer)

### Utilities - ESP8266 Version:
- Arduino IDE v2
- string, iostream, ESP8266WebServer, ESP8266HTTPClient, ESP8266Wifi libraries
- Frontend ((https://freefrontend.com/))
	- Html/Css/Javascript

### Installation of the PC Version:
Guide for installing the program:
- Requirements:
1) Windows
2) Visual Studio 2019

- Guide: 
1) Install the files onto the computer
2) Start Visual Studio 2019
3) Include curl/curl.h in the Webserver.cpp file
4) Go to Project -> Linker -> Additional Dependencies and add the following:
libcurl.lib
Normaliz.lib
Ws2_32.lib
Wldap32.lib
Crypt32.lib
advapi32.lib
5) Change the IP to your own IPv4 address, which you can find by opening command prompt and typing "ipconfig"
6) Compile and run the program
7) The console would show which address the server is started at, just copy it and paste it into the browser.

### Code Samples for Arduino IDE

```c++
void scrapeWebsite(){
  Serial.println("Scraping website " + website);
  http.begin(website);
  int httpCode = http.GET();
  if (httpCode > 0){
      Serial.println("Assigning variable with scraped data:");
      webPage = http.getString();
    }
  //Serial.println(webPage);
}
```

2)
``` c++
String get_my_string(String startDelim, String stopDelim, String myString){
  int firstChar = myString.indexOf(startDelim) + startDelim.length();
  int secondChar = myString.indexOf(stopDelim);

  return myString.substring(firstChar, secondChar);
}

void filterData(){
  token = get_my_string("<img src=\"", "\" class=", webPage);
  actualToken = "<title>" + token + " </title>";
  Serial.println("token: " + token);
  Serial.println("actual token" + actualToken);
}
```
3)
``` c++
void filterData(){
  token = get_my_string("<img src=\"", "\" class=", webPage);
  actualToken = "<title>" + token + " </title>";
  Serial.println("token: " + token);
  Serial.println("actual token" + actualToken);
}

void createWebServer(){
  webServer.send(200, "text/html", actualToken + html);
}
```

4)
``` javascript
	var imglink = "http://www.edoganci.dk/" + document.title;
	var actualImg = new Image();
	var pattern;
	function init()
	  {
	    context = myCanvas.getContext('2d');
	    setInterval(draw, 10);
	    actualImg.src = imglink;
	    actualImg.onload = function(){
	    	pattern = context.createPattern(this, "repeat");
		    context.beginPath();
		    
		    context.fillStyle=pattern;
		    context.arc(100,100,75,0,Math.PI*2,true); context.closePath();
		    context.fill();
	    }
	  }
	function draw()
	{
		 context.clearRect(0,0, canvasX,canvasY);
		 context.beginPath();
		 context.fillStyle = pattern;
		 context.arc(x,y,75,0,Math.PI*2,true);
		 context.closePath();
		 context.fill();
		if( x<0 || x>canvasX) dx=-dx; 
		if( y<0 || y>canvasY) dy=-dy; 
		x+=dx; 
		y+=dy;
	}
```
