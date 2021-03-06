# Setup steps

## Create your own Kubernetes cluster.

At CodeEurope I used a Mac and decided to use minikube to perform the demos.

You can download Minikube on various platforms (Linux, Windows, macOS) and you can download the latest latest version of minikube for your OS from the release page [https://github.com/kubernetes/minikube/releases](https://github.com/kubernetes/minikube/releases).

You will also need to download the kubectl binary.  See [Install and Set Up kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

Alternatively, on Windows 10 and macOS it will be easier to use Docker Desktop which now provides Kubernetes in the stable channel.
You can download Docker Desktop from [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop).  This package also installs the kubectl binary.

These instructions will suppose that you are using minikube.

Type ```minikube start``` to start the cluster, this may take some time especially if it is necessary to download the iso and associated containers over a slow network.

Once finished check that you can access your cluster using kubectl.

Run

```kubectl get nodes```

you should see something like:

<pre>
NAME       STATUS    ROLES     AGE       VERSION
minikube   Ready     master    15m       v1.10.0
</pre>

## Create the "demo dashboard"


### [optional] Installing/running ttyd
To run the dashboard you optionally require the ttyd daemon installed (to allow to type console commands locally in the browser window).

#### Installing ttyd on macOS or Linux
Executables are readily available for Linux and MacOS on the [release page](https://github.com/tsl0922/ttyd/releases).
ttyd is available on macOS via brew.

#### Building ttyd for Windows
On Windows you will first need to install MSYS2 and then compile your own executable, instructions are provided for this [here](https://github.com/tsl0922/ttyd/tree/master/msys2).  I would have made a Windows executable available but following the instructions only created a dynamically linked executable, ... 


Once you have ttyd available launch the executable.

**Note:** we are launching ttyd without any special security ... be careful, you **will** want to investigate use of https, if accessing from another machine.

```
cd live-k8s-visualizer/

launch_ttyd.sh
```

### Running the Visualizer:

#### In a separate terminal window launch the visualizer:

```
cd live-k8s-visualizer/

./visualize.sh
```

### Create the "demo dashboard" page

We will create the dashboard from a template, populated with the URLs

- to the flask-web app (we will need to run the script again later once the app has been deployed).
- to the ttyd shell daemon
- the visualizer

```
cd live-k8s-visualizer/

./create_demo_html.sh
```

**Note**: Initially we cannot set the service port, and so the button shows the URL as something like http://192.168.99.100:PORT_UNSET - see image below.

Later when the service has been started we will rerun create_demo_html.sh with the option '-s' to set the port of the exposed flask-app service.

### Open the "demo dashboard"

You can open the dashboard in your browser either using

- file protocol
- or via a local web server

In my case under Windows I had the files under

- [cygwin path] ```/home/windo/src/git/GIT_mjbright/ConferenceDemo.2018-04_codeeurope-microservices```
- [DOS path] ```C:\tools\cygwin\home\windo\src\git\GIT_mjbright\ConferenceDemo.2018-04_codeeurope-microservices```


#### Accessing the *demo dashboard* using local files:
I can access the *demo dashboard* in my browser using:
```
    file:///C:/tools/cygwin/home/windo/src/git/GIT_mjbright/ConferenceDemo.2018-04_codeeurope-microservices/demo.html
```

Alternatively I could launch a web server, e.g. using Python3, from the codeeurope-microservices repo directory as such:

```
    cd C:/tools/cygwin/home/windo/src/git/GIT_mjbright/ConferenceDemo.2018-04_codeeurope-microservices/
    cd live-k8s-visualizer/

    python3 -m http.server 8000 --bind 127.0.0.1
```

to serve up files just on the same machine, or if you want to access from another machine:
```
cd live-k8s-visualizer/

/usr/bin/python3 -m http.server 8000 --bind 0.0.0.0
```

#### Accessing the *demo dashboard* using a web server:
You can now open your browser at
```http://127.0.0.1:8000/demo.html```

Replace ```127.0.0.1``` by the remote ip if running on a different machine

<img src="images/demo_initial.png" width="800" />
