# cac-auth-example

## What is this?

The manifests in [./deploy/manifests](./deploy/manifests/) can be applied/installed into a local kubernetes cluster to configure a flask application that simply displays the headers that a client sends it; since we also define an Nginx Ingress and a few other resources that enable us to perform mutual TLS (mTLS).

In some environments, such as the DoD, individuals are issued client certificates on Yubikeys or other hardware security modules (HSMs), such as a Smartcard. In the DoD, these smartcards are also known as Common Access Cards (CACs).

We can leverage a user's client certificates to perform a simple authentication/validation check at the Ingress then forward the user's client certificate attributes to a destination application to perform additional authentication/authorization tasks.

Simply put, the manifests in this repository allows us to view these attributes so we know what they look like.

## How to use

First, install/run a local kubernetes cluster. I recommend using [Podman Desktop](https://podman-desktop.io/docs/installation) with the [Kind extension](https://podman-desktop.io/docs/kind).

Next, you'll want to configure an ingress controller. The manifests are written for the Nginx Ingress controller, so I recommend installing/configuring that controller. For kind, use [the command described here](https://kind.sigs.k8s.io/docs/user/ingress#ingress-nginx): `kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml`. Assuming your local kubernetes host is forwarding ports 80/443 to your ingress service, you should now be able to go to `https://localhost` and see a 404 error (after ignoring the warning about an untrusted certificate being presented). We can also leverage a wildcard DNS entry that points to localhost as follows: [https://flask.local.403labs.io](https://flask.local.403labs.io).

Finally, you'll want to run `kubectl apply -f ./deploy/manifests` which will install all of the resources for this example to your local kubernetes cluster. Once the flask pod is running, you should be able to go to [https://flask.local.403labs.io](https://flask.local.403labs.io) and flask should now present you with the headers it's receiving. If you have a CAC, you'll be presented with a system/browser dialog box to choose a client certificate; once you enter your pin, Nginx will parse and forward your headers to Flask and you should see those.

## How does this work?

TODO
