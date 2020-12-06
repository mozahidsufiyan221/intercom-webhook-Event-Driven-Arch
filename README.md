# intercom-webhook-Event-Driven-Arch

https://mozahidsufiyan222-96951.medium.com/eda-event-driven-architecture-integrate-intercom-webhook-via-ngrok-socket-python-flask-756568ef1fe0 


#Intercom is quite a familiar app used in the product industry, a place where you can interact with your clients. Intercom lets you play with your data through its webhook feature. You can forward data to your favorite apps or add more business logic. You are seven steps away from your first integration. Get ready to start a new journey to the world of automation.
Below is a screenshot of a completed project. This tutorial will direct you to build up an operational client application. So you can customize your web page to your requirements.
Python Ngrok configuration
Before starting the tutorial you should have to make sure your machine is fulfilling the following requirements.
Python 3.6.7, Pip Install flask
Ngrok (Ngrok secure introspectable tunnels to localhost webhook development tool and debugging tool.)
Intercom Account Signed Up (Developers Hub webhooks.)
Here is the project repository stored in GitHub. Clone the project into your local machine. And open it up with your favorite IDE. For this tutorial, I have used JetBrainPycharm.
https://github.com/mozahidsufiyan221/intercom-webhook-Event-Driven-Arch
Before starting the configurations you should have some basic idea regarding the following properties.
You will need an endpoint to configure a webhook as explained in this article. Lets get an endpoint instantly from socket.
Let’s start the Ngrok configurations.
Download Ngrok file
Please download Ngrok from the site below for windows.
Image for post
Connect your Ngrok account
Running this command will add your auth token to the default ngrok.yml configuration file. This will grant you access to more features and longer session times. Running tunnels will be listed on the status page of the dashboard.
$ ngrok authtoken 1l4Pz3EQAYufyrrHKYBAo4yQoDg(may vary in your case)
Write Python to Handle the Incoming Messages
Now comes the fun part-writing code that will handle an incoming HTTP request from Intercom! Our code will dictate what happens when our messages receive by responding with Intercom.
In production, you probably have a public URL, but you probably don’t during development. That’s where ngrok comes in. ngrok gives you a public URL for a local port on your development machine, which you can use to configure your Intercom webhooks as described above.
Once Ngrok is installed, you can use it at the command line to create a tunnel to whatever port your web application is running on. For example, this will create a public URL for a web application listening on port 5000.
ngrok http 5000
After executing that command, you will see that Ngrok has given your application a public URL that you can use in your webhook configuration in the Intercom developer hub console.
Image for post
Grab your Ngrok public URL and head back to the phone number you configured earlier. Now let’s switch it from using an Intercom to use your new ngrok URL. Don’t forget to append the URL path to your actual Intercom logic! (“https://<your ngrok subdomain>.ngrok.io/webhook” for example)
Bridge the URL
You should have to run it with ngork to locally check it. So that, Open the terminal and run ngrok http 5000 command. So the terminal will display ngrok forwarded URL. Copy it and navigate to it from the browser. Refer the following screenshot.
Image for post
ngrok session status
so take e.g https://33be109c3ce2.ngrok.io from above and paste below within Intercom, in your case different HTTPS link will provided by Ngrok
Intercom application configurations
Webhook Description
This is the final step. All the application side changes are almost over. Now you should have to set up the Intercom configurations. Paste HTTPS link provided by Ngrok on the below-given description on phone number and scroll down and you will see the voice and update the voice URL with ngrok forwaded URL + /webhook.
Socket setup sample
2. Now go to Intercom and find the webhook option
settings -> app setting -> developer tools -> webhooks
3. Create a webhook
4. Paste socket URL (endpoint) into the Intercom webhook configuration.
Image for post
Intercom webhook configuration
5. Select events topic when you want to receive data from Intercom.
6. Conduct the test by trying that event. You will see some data in Socket like this —
Image for post
I have selected two variables as an example (you can see those highlighted) on which you want to perform task.
Create Python responses to incoming messages
In the example above, we returned a pre-defined Intercom in response to the incoming messages. The real power of using webhooks like this is executing dynamic code (based on the information Intercom sends to your application) to change what you present to the user on the other end of the messages. You could query your database, reference a customer’s information in your CRM, or execute any kind of custom logic before determining how to respond to your user.
from flask import Flask, request, abort
app = Flask(__name__)
@app.route('/webhook', methods=['POST'])
def webhook():
    if request.method == 'POST':
        print(request.json)
        return '', 200
    else:
        abort(400)
if __name__ == '__main__':
    app.run()
Hopefully, Now your application is running without issue. If you faced any issues post those in the comment section.
Image for post
References
What is a Webhook?
Webhooks are user-defined HTTP callbacks. They are usually triggered by some event, such as receiving an SMS message or an incoming phone call. When that event occurs, the Intercom makes an HTTP request (usually a POST or a GET) to the URL configured for the webhook.
In most web frameworks, there is the concept of a route. A route allows the application to respond with specific content or data when the user visits a specific URL. The same idea applies to APIs.
The same concept is applied when receiving webhooks. We establish a route, tell the service where to send the data, and our application sits and waits until a request comes into that route.
There are a few consistencies across webhook implementations.
They are normally POST requests.
They receive JSON data.
They need to respond quickly.
