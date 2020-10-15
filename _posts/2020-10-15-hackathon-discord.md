---
title: "Make a discord bot for qadjokes"
categories: 
  - Hackathon
tags:
  - Hackathon
  - Qiskit
  - Discord
  - Heroku
last_modified_at: 2020-10-15
author_profile: true
---
I participated in Qiskit Hackathon Global, from October 5 to October 10.<br/>
The entire code can be found in the github repository shared: [quantum-ugly-duckling](https://github.com/rochisha0/quantum-ugly-duckling).

In this project, I made a website which is linked to the database with IBM Cloud.<br/>
The database is for saving quantum-dadjoke, in short, qadjokes.<br/>
Then, connect the database to the discord bot printing qadjokes.<br/>

I used [Goorm IDE](https://ide.goorm.io/) for making things above.<br/>

## Make a website with Cloud Foundry

Go to [IBM Cloud](https://cloud.ibm.com/login) and type `Cloud Foundry` in the search bar.<br/>
Then make an app with Python. (I chose Python.)

```
git clone https://github.com/IBM-Cloud/get-started-python
```
```
cd get-started-python
```

Change `name` in `manifest.yml` to your Cloud Foundry app name.<br/>
I used nano to modify the file.

```
sudo apt-get install nano
```

Install `ibmcloud` command.

```
curl -sL https://ibm.biz/idt-installer | bash
```

![after](https://user-images.githubusercontent.com/62553200/96062970-e651b400-0ed1-11eb-9af3-9555a10729fa.png)

You can see the screen like this if installation succeeded.

Change HTML file for your website.

```
cd static
```
```
nano index.html
```

You can check our `index.html` codes here:
[quantum-ugly-duckling/quantum-ugly-duckling-main/static/index.html](https://github.com/rochisha0/quantum-ugly-duckling/blob/main/quantum-ugly-duckling-main/static/index.html)

Then, come back to the main folder and let's deploy it.

```
cd ..
```
```
ibmcloud target --cf
```
```
cf login
```
```
cf push
```

It takes time. Please wait a moment!

## Connect a database to the website

Come back to [IBM Cloud](https://cloud.ibm.com/login) and type `Cloudant` in the search bar.<br/>
While making, it is important to check `IAM and legacy credentials` in Authentication method.

In resource list, go to Cloud Foundry and click `connection`.<br/>
Then, you can see the blue button with `Create connection`.<br/>
Click it and connect with Cloudant without any setting changes.<br/>

After a while, `Restage app` will pop up. Click `Restage`.

---

Get credentials from Cloudant and open `vcap-local.template.json`.

![vcap-local](https://user-images.githubusercontent.com/62553200/96067604-1e0e2b00-0ed5-11eb-896c-8e259975592b.png)

Change the part in `"credentials"` with the credentials and save the file with `vcap-local.json`.<br/>

---

Open `hello.py` and add a function below.<br/>

```python
@app.route('/index.html', methods=['GET'])
def save_two():
    Qdad = request.args.get('Qdad')
    data = {'Qdad': Qdad}
    if client:
        my_document = db.create_document(data)
        data['_id'] = my_document['_id']
    else:
        print('No database')
    return app.send_static_file('index.html')
```

Check also `form` tag in `index.html`.

```
<form action="./index.html">
```

## Make a discord bot and host with Heroku

You can check our `discord_bot.py` codes here:
[quantum-ugly-duckling/quantum-ugly-duckling-main/discord_bot.py](https://github.com/rochisha0/quantum-ugly-duckling/blob/main/quantum-ugly-duckling-main/discord_bot.py)<br/>

We choose [Heroku](https://www.heroku.com/) for hosting our bot.<br/>
For uploading codes, we used Heroku CLI.<br/>
Install it using the code below.

```
curl https://cli-assets.heroku.com/install-ubuntu.sh | sh
```

After installing it, follow the codes below.<br/>

### ❗ Before following

Check that 4 files are in the folder.<br/>

1. [config.json](https://github.com/rochisha0/quantum-ugly-duckling/blob/main/quantum-ugly-duckling-main/config.json)
2. [requirements.txt](https://github.com/rochisha0/quantum-ugly-duckling/blob/main/quantum-ugly-duckling-main/requirements.txt)
3. [Procfile](https://github.com/rochisha0/quantum-ugly-duckling/blob/main/quantum-ugly-duckling-main/Procfile)
4. [runtime.txt](https://github.com/rochisha0/quantum-ugly-duckling/blob/main/quantum-ugly-duckling-main/runtime.txt)

If `runtime.txt` doesn't exist, you can meet `RuntimeError`.

![error](https://user-images.githubusercontent.com/62553200/96069480-439d3380-0ed9-11eb-9232-e7eaa688217e.png)

If you are clear that all files are there, you can follow the codes below.<br/>

```
heroku login -i
```

```
heroku git:clone -a <your app name>
cd <your app name>
```

```
git add .
git commit -am "<commit message>"
git push heroku master
```

Now you can see your bot is working well!

![working](https://github.com/rochisha0/quantum-ugly-duckling/blob/main/images/discord_test.gif?raw=true)

If you are interested in our qadjoke bot, invite our lovely duck to your Discord server!
- [Click it and invite Qadjoke bot!](https://discord.com/api/oauth2/authorize?client_id=763802062370111498&permissions=67584&scope=bot)
- [Official Qadjoke bot server](https://discord.gg/3UdGBAC)

---

💬 *Any comments and suggestions will be appreciated.*
