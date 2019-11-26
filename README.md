<div align="center"><h1>Actions Connect Open VPN</h1></div>

This action is a connect ovpn script

## Example file `.ovpn` to connect vpn

[Test.ovpn](./test.ovpn)

## Configuration

The following settings must be passed as environment variables as shown in the
example.

| Key         | Value                                                                                                                           | Suggested Type | Required | Default |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------- | -------------- | -------- | ------- |
| `FILE_OVPN` | Location file open vpn and .                                                                                                    | `env`          | **Yes**  | N/A     |
| `PING_URL`  | URL for check status vpn connect pass or fail                                                                                   | `env`          | **Yes**  | N/A     |
| `CA_CRT`    | Certificate for access vpn `(Encode base 64 before set secret.)`[How to encode base 64 ?](https://www.base64encode.org/).       | `secret env`   | **Yes**  | N/A     |
| `USER_CRT`  | User certificate for access vpn. `(Encode base 64 before set secret.)`[How to encode base 64 ?](https://www.base64encode.org/). | `secret env`   | **Yes**  | N/A     |
| `USER_KEY`  | User key for access vpn. `(Encode base 64 before set secret.)`[How to encode base 64 ?](https://www.base64encode.org/).         | `secret env`   | **Yes**  | N/A     |
| `USERNAME`  | Username for access vpn.                                                                                                        | `secret env`   | No       | N/A     |
| `PASSWORD`  | Password for access vpn.                                                                                                        | `secret env`   | No       | N/A     |
| `TLS_KEY`   | Tls-crypt for access vpn `(Encode base 64 before set secret.)`[How to encode base 64 ?](https://www.base64encode.org/).         | `secret env`   | No       | N/A     |

## Outputs

### `STATUS`

**Boolean** Can get status after connect `true` or `false`.

## Example usage

```yml
- name: Install Open VPN
  run: sudo apt-get install openvpn
- name: Connect VPN
  uses: golfzaptw/action-connect-ovpn@master
  id: connect_vpn
  with:
    PING_URL: '127.0.0.1'
    FILE_OVPN: 'config.ovpn'
    CA_CRT: ${{ secrets.CA_CRT}}
    USER_CRT: ${{ secrets.USER_CRT }}
    USER_KEY: ${{ secrets.USER_KEY }}
    USERNAME: ${{ secrets.USERNAME }}
    PASSWORD: ${{ secrets.PASSWORD }}
    TLS_KEY: ${{ secrets.TLS_KEY }}
- name: Check Connect VPN
  run: echo ${{ steps.connect_vpn.outputs.STATUS }}
```
