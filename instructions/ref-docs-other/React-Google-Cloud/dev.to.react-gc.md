Copied from
https://dev.to/cormacncheese/deploying-an-expo-web-react-native-app-to-google-cloud-4006

Building mobile apps has never been easier thanks to Expo. According to their website, Expo is an open-source platform for making universal native apps for Android, iOS, and the web with JavaScript and React.

Paired with Google Cloud Run, highly scalable containerized a fully managed serverless platform, Expo web apps can be extremely quick to make and ship.

Here’s how to build and deploy your React Native Expo web app to Google Cloud Run:

0) Setup Expo
   I’m assuming you already have an Expo app setup and running locally (their docs are super easy to follow)

1) Add a Dockerfile to the root of your application
   You can copy and paste this one into your project:

`# build environment
FROM node:14-alpine as react-build

install global packages
ENV NPM_CONFIG_PREFIX=/home/node/.npm-global
ENV PATH /home/node/.npm-global/bin:$PATH
RUN npm i --unsafe-perm -g npm@latest expo-cli@latest
WORKDIR /app
ADD . ./
RUN yarn
RUN npx expo-optimize

used for ENV
RUN expo build:web --release-channel prod

server environment
FROM nginx:alpine
ADD nginx.conf /etc/nginx/conf.d/configfile.template
COPY --from=react-build /app/web-build /usr/share/nginx/html
ENV PORT 8080
ENV HOST 0.0.0.0
EXPOSE 8080
CMD sh -c "envsubst '\$PORT' < /etc/nginx/conf.d/configfile.template > /etc/nginx/conf.d/default.conf && nginx -g 'daemon off;'"`

The key elements here which are super important is making sure to expo-cli as well as including the RUN npx expo-optimize and RUN expo build:web — release-channel prod commands.

These include all the necessary Expo packages and scripts.

3. Setup Google Cloud
   You’ll need to make sure you’ve enabled Cloud Build, Cloud Run and Container Registry in your project.

You’ll also need to sign into Gcloud from the command line:

gcloud auth login
gcloud config set project YOUR_PROJECT_ID

Build using Docker
gcloud builds submit --timeout=2000s --machine-type=N1_HIGHCPU_8 --tag gcr.io/YOUR_PROJECT_ID/frontend

You can remove the machine-type=N1_HIGHCPU_8 tag if you want to use the starter level server. Make sure to add your project ID.

The cli will prompt you to enter a project name

5. Submit build to Google Cloud Run
   gcloud run deploy YOUR_PROJECT_NAME —image gcr.io/mortar-329801/frontend --region us-west2 --platform managed

Feel free to change the region.

And your app is live!