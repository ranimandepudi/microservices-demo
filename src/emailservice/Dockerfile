# Copyright 2020 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# syntax=docker/dockerfile:1.6
ARG TARGETPLATFORM
FROM --platform=$TARGETPLATFORM python:3.12.8-alpine@sha256:54bec49592c8455de8d5983d984efff76b6417a6af9b5dcc8d0237bf6ad3bd20 AS base
FROM --platform=$TARGETPLATFORM base AS builder
RUN apk add --no-cache g++ gcc linux-headers musl-dev libffi-dev openssl-dev
COPY requirements.txt .
RUN --mount=type=cache,target=/root/.cache/pip \
    pip install --prefer-binary -r requirements.txt
FROM --platform=$TARGETPLATFORM base
ENV PYTHONUNBUFFERED=1
ENV ENABLE_PROFILER=1
RUN apk add --no-cache libstdc++ libgcc libffi openssl
WORKDIR /email_server
COPY --from=builder /usr/local/lib/python3.12/ /usr/local/lib/python3.12/
COPY . .
EXPOSE 8080
ENTRYPOINT ["python","email_server.py"]



