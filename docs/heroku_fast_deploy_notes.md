# quickly start a react app and deploy with git and heroku 

```
npm install -g create-react-app
create-react-app my-app
cd my-app
git init
heroku create -b https://github.com/mars/create-react-app-buildpack.git
git add .
git commit -m "react-create-app on Heroku"
git push heroku master
heroku open
```

any more changes? simply do 
```
git add . 
git commitm -m "more changes added "
git push heroku master
```
you can add as many remote you want :) before that make sure you have created a app using 
heroku apps:create another-version-of-your-app # this is just another name 

