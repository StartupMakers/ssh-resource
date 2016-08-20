# SSH Resource

Executes specified commands on a remote host using SSH.

## Source Configuration

* `host`: *Required.* The target host to connect.

* `username`: *Required.* The username which will be used.

* `password`: *Optional.* Password for specified username.

* `private_key`: *Optional.* Private key to use when connecting. Example:

    ```yaml
    private_key: |
      -----BEGIN RSA PRIVATE KEY-----
      MIIEowIBAAKCAQEAtCS10/f7W7lkQaSgD/mVeaSOvSF9ql4hf/zfMwfVGgHWjj+W
      <Lots more text>
      DWiJL+OFeg9kawcUL6hQ8JeXPhlImG6RTUffma9+iGQyyBMCGd1l
      -----END RSA PRIVATE KEY-----
    ```

### Example

Resource configuration for a private repo:

```yaml
resources:
- name: source-code
  type: git
  source:
    host: my-private.com
    username: docker_user
    private_key: |
      -----BEGIN RSA PRIVATE KEY-----
      MIIEowIBAAKCAQEAtCS10/f7W7lkQaSgD/mVeaSOvSF9ql4hf/zfMwfVGgHWjj+W
      <Lots more text>
      DWiJL+OFeg9kawcUL6hQ8JeXPhlImG6RTUffma9+iGQyyBMCGd1l
      -----END RSA PRIVATE KEY-----
```

Connect to the server and re-start Docker container:

```yaml
- name: deploy
  plan:
    - get: web-image
      trigger: true
      passed: [build]
    - put: target-server
      params:
        command: 'docker stop my-app; docker rm -v -f my-app; docker pull my-private.com:5000/my-app && docker run -d -p 80:80 --restart=always --name my-app my-private.com:5000/my-app'
```

## Behavior

### `out`: Execute specified command on a remote server

#### Parameters

* `command`: *Required.*

## Thanks

Originally based on [git-resource](https://github.com/concourse/git-resource) extension by Concourse team.
