# Firebase-Count
This project showcases how to use the ESP8266 microcontroller to upload string data to Firebase. It involves a simple push button interface that increments a counter each time the button is pressed.
# Aim
To interface a push button with the ESP8266 microcontroller and upload the button count to Firebase after converting the integer count to a string without using Pre-defined functions
# Components
1. Esp8266
2. Push Button
# connection
![lesson7](https://github.com/user-attachments/assets/58516281-9c48-452e-a965-11312b01ed4f)
# Procedure 
1. Create a Firebase Project
2. Collect The network credentials, URL database, and project API key ,Email ,Password
3. Create a ESP8266 Projec On  Visual Studio Code With Arduino FrameWork
4. Interface ESP8266 microcontroller with push button and print the digital values on serial monitor.
5. Install the Firebase-ESP-Client Library.Then, program the ESP8266 to Interface with Firebase
   * lib_deps = mobizt/Firebase Arduino Client Library for ESP8266 and ESP32@^4.4.14
6. Insert your network credentials To the project  work.
# Code With out Pre-defined functions
## 1. Method
```
    #if defined(ESP32)
    #include <WiFi.h>
    #elif defined(ESP8266)
    #include <ESP8266WiFi.h>
    #endif
    #include <Firebase_ESP_Client.h>
    #include <addons/TokenHelper.h>
    #include <addons/RTDBHelper.h>
    #define WIFI_SSID "-----------------"
    #define WIFI_PASSWORD "------------------"
    #define API_KEY "--------------------------------------------"
    #define DATABASE_URL "---------------------------------------"
    #define USER_EMAIL "-----------------"
    #define USER_PASSWORD "---------"
    FirebaseData fbdo;
    FirebaseAuth auth;
    FirebaseConfig config;
----------------------------------------------------------------------------------------------------------------
    int count = 0 ;
    String msg;
----------------------------------------------------------------------------------------------------------------
    void Connect_WiFi();
    void Firebase_Store(String PATH,String MSG);
    String Firebase_getString(String PATH);
    int key();
    void uploadVal(int);
----------------------------------------------------------------------------------------------------------------
    void setup()
    {
         Connect_WiFi(); 
         pinMode(D2,INPUT);
    }
----------------------------------------------------------------------------------------------------------------
    void loop()
    {
      int k;
      k=key();
      if(k==1)
      {
        count ++;
        Serial.println(count);
        uploadVal(count);
      }
    }
----------------------------------------------------------------------------------------------------------------
    void Connect_WiFi()
    {
          Serial.begin(9600);
          delay(100); 
          WiFi.disconnect();
          delay(800); 
          Serial.println("Connecting to Wi-Fi"); 
          WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
          Serial.print("Connecting to Wi-Fi");
          delay(100);
          while (WiFi.status() != WL_CONNECTED)
          {
            Serial.print(".");
            delay(300);
          }
          Serial.println();
          Serial.print("Connected with IP: ");
          Serial.println(WiFi.localIP());
          Serial.println();
          Serial.printf("Firebase Client v%s\n\n", FIREBASE_CLIENT_VERSION);
          config.api_key = API_KEY;
          auth.user.email = USER_EMAIL;
          auth.user.password = USER_PASSWORD;
          config.database_url = DATABASE_URL;
          config.token_status_callback = tokenStatusCallback;
           #if defined(ESP8266)
            fbdo.setBSSLBufferSize(2048 /* Rx buffer size in bytes from 512 - 16384 */, 2048 /* Tx buffer size in bytes from 512 - 16384 */);
          #endif
          Firebase.begin(&config, &auth);
          Firebase.reconnectWiFi(true);
          Firebase.setDoubleDigits(5);
          config.timeout.serverResponse = 10 * 1000;
    }
----------------------------------------------------------------------------------------------------------------
    void Firebase_Store(String PATH,String MSG)
    {
          Serial.print("Uploading data \" ");
          Serial.print(MSG);
          Serial.print(" \"  to the location \" ");
          Serial.print(PATH);
          Serial.println(" \"");
          Firebase.RTDB.setString(&fbdo, PATH, MSG);
          delay(50);
    }
----------------------------------------------------------------------------------------------------------------
    String Firebase_getString(String PATH)
    {
      String msg = (Firebase.RTDB.getString(&fbdo, PATH) ? fbdo.to<const char *>() : fbdo.errorReason().c_str());
      delay(50);
      return msg;
    }
----------------------------------------------------------------------------------------------------------------
    int key()
    {
      int x=0;
      while(digitalRead(D2)==0)
      {
        x=1;
        delay(50);
      }
      return x;
    }
----------------------------------------------------------------------------------------------------------------
    void uploadVal(int n)
    {
        int r,a[10],i=0;
        while(n>0)
        {
          r=n%10;
          n=n/10;
          a[i]=r;
          i++;
        }
        msg ="";
        for(i--;i>=0;i--)
        {
            if(a[i]==0)
            {
              msg += "0";
            }
            else if(a[i]==1)
            {
              msg += "1";// msg =msg+ "1"
            }
            else if(a[i]==2)
            {
              msg += "2";
            }
            else if(a[i]==3)
            {
              msg += "3";
            }
            else if(a[i]==4)
            {
              msg += "4";
            }
            else if(a[i]==5)
            {
              msg += "5";
            }
            else if(a[i]==6)
            {
              msg += "6";
            }
            else if(a[i]==7)
            {
              msg += "7";
            }
            else if(a[i]==8)
            {
              msg += "8";
            }
            else if(a[i]==9)
            {
              msg += "9";
            }
        }
        Serial.println(msg);
        Firebase_Store("/test/count",msg);
    }
```
## 2. Method
```
    #if defined(ESP32)
    #include <WiFi.h>
    #elif defined(ESP8266)
    #include <ESP8266WiFi.h>
    #endif
    #include <Firebase_ESP_Client.h>
    #include <addons/TokenHelper.h>
    #include <addons/RTDBHelper.h>
    #define WIFI_SSID "-----------------"
    #define WIFI_PASSWORD "------------------"
    #define API_KEY "--------------------------------------------"
    #define DATABASE_URL "---------------------------------------"
    #define USER_EMAIL "-----------------"
    #define USER_PASSWORD "---------"
    FirebaseData fbdo;
    FirebaseAuth auth;
    FirebaseConfig config;
----------------------------------------------------------------------------------------------------------------
    int count = 0 ;
    int c=0,num;
    String msg;
    
----------------------------------------------------------------------------------------------------------------
    int cnt();
    char digit1(int);
    void Connect_WiFi();
    void Firebase_Store(String PATH,String MSG);
    String Firebase_getString(String PATH);

----------------------------------------------------------------------------------------------------------------
    void setup()
    {
         Connect_WiFi(); 
         pinMode(D2,INPUT);
    }

----------------------------------------------------------------------------------------------------------------
void loop()
    {
         int x,r,l,i=0,j;
         char s[10];
         x=cnt();
         if(x==1)
         {
            c++;
            num=c;
            Serial.println(c);
            msg="";
            while(c>0)
            {
              r=c%10;
              c=c/10;
             s[i] =digit1(r);
             i++;
            }
            for(i--;i>=0;i--)
            {
              msg +      = s[i];
            }
            Serial.println(msg);
          Firebase_Store("/PUSHCOUNT/DATA",msg);
          c=num;
         }

    }

----------------------------------------------------------------------------------------------------------------
    void Connect_WiFi()
    {
          Serial.begin(9600);
          delay(100); 
          WiFi.disconnect();
          delay(800); 
          Serial.println("Connecting to Wi-Fi"); 
          WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
          Serial.print("Connecting to Wi-Fi");
          delay(100);
          while (WiFi.status() != WL_CONNECTED)
          {
            Serial.print(".");
            delay(300);
          }
          Serial.println();
          Serial.print("Connected with IP: ");
          Serial.println(WiFi.localIP());
          Serial.println();
          Serial.printf("Firebase Client v%s\n\n", FIREBASE_CLIENT_VERSION);
          config.api_key = API_KEY;
          auth.user.email = USER_EMAIL;
          auth.user.password = USER_PASSWORD;
          config.database_url = DATABASE_URL;
          config.token_status_callback = tokenStatusCallback;
          #if defined(ESP8266)
            fbdo.setBSSLBufferSize(2048 /* Rx buffer size in bytes from 512 - 16384 */, 2048 /* Tx buffer size in bytes from 512 - 16384 */);
          #endif
          Firebase.begin(&config, &auth);
          Firebase.reconnectWiFi(true);
          Firebase.setDoubleDigits(5);
          config.timeout.serverResponse = 10 * 1000;
    }

----------------------------------------------------------------------------------------------------------------
    void Firebase_Store(String PATH,String MSG)
    {
          Serial.print("Uploading data \" ");
          Serial.print(MSG);
          Serial.print(" \"  to the location \" ");
          Serial.print(PATH);
          Serial.println(" \"");
          Firebase.RTDB.setString(&fbdo, PATH, MSG);
          delay(50);
    }

----------------------------------------------------------------------------------------------------------------
    String Firebase_getString(String PATH)
    {
      String msg = (Firebase.RTDB.getString(&fbdo, PATH) ? fbdo.to<const char *>() : fbdo.errorReason().c_str());
      delay(50);
      return msg;
    }

----------------------------------------------------------------------------------------------------------------
   int cnt()
    {
      int y=0;
      while(digitalRead(D2)==0)
      {
        y=1;
        delay(50);
      }
      return y;
    }

----------------------------------------------------------------------------------------------------------------
char digit1(int x)
    {
      switch(x)
      {
        case 0: return '0';
        break;
        case 1: return '1';
        break;
        case 2: return '2';
        break;
        case 3: return '3';
        break;
        case 4: return '4';
        break;
        case 5: return '5';
        break;
        case 6: return '6';
        break;
        case 7: return '7';
        break;
        case 8: return '8';
        break;
        case 9: return '9';
        break;
      }
     return 0;
    }
----------------------------------------------------------------------------------------------------------------
```
Output
![WhatsApp Image 2024-09-02 at 11 47 55 AM](https://github.com/user-attachments/assets/a7dbd6b2-36b3-4ae2-bb84-bde4de728ac6)
![WhatsApp Image 2024-09-02 at 11 48 23 AM](https://github.com/user-attachments/assets/9f3893ad-f90b-447e-9ce6-e7c5b1dfe48f)
# Visual Studio Code Installation
Link = https://code.visualstudio.com/download
