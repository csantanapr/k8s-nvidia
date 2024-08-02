# Jetson Containers

Machine Learning Containers for Jetson and JetPack

https://github.com/dusty-nv/jetson-containers

Install jetson-containers following https://github.com/dusty-nv/jetson-containers/blob/master/docs/setup.md

kubectl run tmp-shell --rm -i --tty --overrides='{"spec": {"hostNetwork": true}}'  --image nicolaka/netshoot

jetson-containers run dustynv/cuda:12.2-samples-r36.2.0 /opt/cuda-samples/bin/aarch64/linux/release/deviceQuery

docker run dustynv/cuda:12.2-samples-r36.2.0 /opt/cuda-samples/bin/aarch64/linux/release/deviceQuery

kubectl run tmp-shell --rm -i --tty --image dustynv/cuda:12.2-samples-r36.2.0 -- /opt/cuda-samples/bin/aarch64/linux/release/deviceQuery