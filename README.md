# kube-gcp-disks-roomba-chart

Helm chart for https://github.com/softonic/kube-gcp-disks-roomba

[![Latest Version](https://img.shields.io/github/release/softonic/kube-gcp-disks-roomba.svg)](https://github.com/softonic/kube-gcp-disks-roomba/releases)
[![Software License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![Average time to resolve an issue](http://isitmaintained.com/badge/resolution/softonic/kube-gcp-disks-roomba.svg)](http://isitmaintained.com/project/softonic/kube-gcp-disks-roomba "Average time to resolve an issue")
![GitHub issues](https://img.shields.io/github/issues-raw/softonic/kube-gcp-disks-roomba)


### Overview


| Parameter                                           | Description                                                   | Default                            |
| --------------------------------------------------- | ------------------------------------------------------------- | ---------------------------------- |
| `args`                                              | Array of GCP zones to check                                   | `null`                             |
| `image.tag`                                         | Tag used for the image                                        | `latest`                           |
| `resources`                                         | Nested hash of chart resources                                | `null`                             |
| `secretKey`					      | If the google auth file is saved in a different secret key    | `key.json`                         | 


If the google auth file is saved in a different secret key you can specify it here
secretKey: key.json

### Usage

YOU SHOULD PROVIDE A SECRET NAME CALLED credentials. 
And should contain the json service account credentials that will have the permissions to delete the unused disks in GCP.

If we want this chart to work, we need to set the following secrets:

- credentials:

Will be a secret mount with the contents of the environment variable GOOGLE_APPLICATION_CREDENTIALS

This is an extract of cronjob yaml definition

```
            volumeMounts:
            - name: google-cloud-key
              mountPath: /secrets
            env:
            - name: GOOGLE_APPLICATION_CREDENTIALS
              value: /secrets/key.json
            {{- end }}
          {{- if .Values.secret }}
          volumes:
            - name: google-cloud-key
              secret:
                secretName: {{ .Values.secret }}
                items:
                  - key: {{ default "key.json" .Values.secretKey }}
                    path: key.json
          {{- end }}

```
