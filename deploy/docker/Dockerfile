# Copyright 2017 The Kubernetes Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Update only after new version of deploy/base/Dockerfile change has built
ARG GANESHA_VERSION=V4.0.8

# from go builder debian image to build nfs-provisioner
FROM golang:1.16.3-buster as builder

# install make
RUN apt-get update && apt-get install -y make

# copy source code
COPY . /go/nfs-ganesha-server-and-external-provisioner

# build nfs-provisioner
RUN cd /go/nfs-ganesha-server-and-external-provisioner \
    && make build

# nfs-provisioner image
FROM ghcr.io/kubernetes-sigs/nfs-ganesha:${GANESHA_VERSION}

# copy nfs-provisioner binary from builder image
COPY --from=builder /go/nfs-ganesha-server-and-external-provisioner/bin/nfs-provisioner /nfs-provisioner

# expose mountd 20048/tcp and nfsd 2049/tcp and rpcbind 111/tcp 111/udp
EXPOSE 2049/tcp 20048/tcp 111/tcp 111/udp

ENTRYPOINT ["/nfs-provisioner"]
