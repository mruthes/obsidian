  


![](https://miro.medium.com/v2/resize:fit:679/1*wzELoJDpKdRAyR0IDeeeVA.png)

To trigger a Jenkins job based on a change in a Jira ticket for **free**, we can set up a system to accomplish this using a combination of webhooks and scripts. Here’s a step-by-step guide:

1. Configure a webhook in Jira:

a. Log in to Jira as an administrator.  
b. Click on "Settings" (gear icon) in the top right corner.  
c. Select "System" from the dropdown menu.  
d. Scroll down to the "Advanced" section and click "Webhooks."  
e. Click on "Create a webhook" and fill in the details like name, URL, and events you want to trigger the webhook (e.g., issue updated, the issue created).  
f. Save the webhook.

**For more steps on JIRA webhook creation use the link:**

[https://community.atlassian.com/t5/Jira-articles/JIRA-WebHooks/ba-p/1569244#:~:text=It%20is%20a%20user%2Ddefined,certain%20events%20occur%20in%20Jira.&text=If%20you%20need%20to%20test,event%20sent%20out%20by%20Jira](https://community.atlassian.com/t5/Jira-articles/JIRA-WebHooks/ba-p/1569244#:~:text=It%20is%20a%20user%2Ddefined,certain%20events%20occur%20in%20Jira.&text=If%20you%20need%20to%20test,event%20sent%20out%20by%20Jira).

2. Create a lightweight server to receive the webhook events: a. You can use a simple Python script with Flask to create a server that listens for incoming webhook events from Jira. b. Install Flask if you haven’t already (`pip install Flask`). c. Create a script to handle incoming events and trigger the Jenkins job. Here's a sample script:

`from flask import Flask, request`  
`import requests`

`app = Flask(__name__)JENKINS_URL = "http://your-jenkins-url.com"`  
`JENKINS_JOB_NAME = "your-job-name"`  
`JENKINS_API_TOKEN = "your-jenkins-api-token"`  
`JENKINS_USER = "your-jenkins-username"@app.route('/jira-webhook', methods=['POST'])`  
`def jira_webhook():`  
    `payload = request.get_json()    # You can add custom logic here to filter specific events or tickets.`  
    `# Example: if payload['issue']['key'] == "JIRA-123":    trigger_jenkins_job()`  
    `return "OK", 200def trigger_jenkins_job():`  
    `job_url = f"{JENKINS_URL}/job/{JENKINS_JOB_NAME}/build"`  
    `auth = (JENKINS_USER, JENKINS_API_TOKEN)`  
    `response = requests.post(job_url, auth=auth)    if response.status_code == 201:`  
        `print("Jenkins job triggered successfully.")`  
    `else:`  
        `print(f"Failed to trigger Jenkins job. Status code: {response.status_code}")if __name__ == '__main__':`  
    `app.run(host='0.0.0.0', port=5000)`

3. Update the Jira webhook URL to point to your new server (e.g., `[http://your-server-ip:5000/jira-webhook](http://your-server-ip:5000/jira-webhook).)`[).](http://your-server-ip:5000/jira-webhook).)

Test the webhook URL on the site:https://webhook.site/

4. Run the Python script on your server to start listening for incoming webhook events.

Now, when there is a change in a Jira ticket, the webhook will send an event to your server, which will trigger the Jenkins job based on your custom logic.

[[JIRA]]
[[Jenkins]]
