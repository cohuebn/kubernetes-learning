install:
	k3d cluster create --config ./cluster.yaml
	istioctl install --skip-confirmation

clean:
	k3d cluster delete --config ./cluster.yaml