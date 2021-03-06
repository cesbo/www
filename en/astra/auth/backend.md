# HTTP Backend

The HTTP Backend implements client authorization based on the result of a subrequest. If the subrequest returns a 200 response code, the access is allowed. If it returns 4xx, the access is denied.

## Configuration

Configuration is simple just select "HTTP Request" as a backend type in the Settings -> HTTP Auth. And set backend absolute address.

## Session information

Astra sends next HTTP headers to the backend:

- `X-Session-ID` - unique session number;
- `X-Channel-ID` - unique channel identifier;
- `X-Real-IP` - client's IP address;
- `X-Real-Path` - client's request path;
- `X-Real-UA` - client's User-Agent;
- `X-Real-Host` - client's Host request.

In a response backend may send next HTTP headers:

- `X-Session-Name` - client login or any other name for session;
- `X-Location` - redirect client to defined location.

## Query string

Query string is a part of address after `?` symbol. Astra appends client's query string to the backend absolute address.

## Example

For example:

1. Your backend address is: `https://auth.example.com/check`;
2. Client tries to start channel: `https://live.example.com/play/a001/index.m3u8?token=123`;
3. Full address to HTTP backend will be: `https://auth.example.com/check?token=123`;
4. In headers will be `X-Real-Path: /play/a001/index.m3u8` and other headers depending of clients request.

## Default action

If backend is not available, then Astra allows access.

## Troubleshooting

### Unexpected access

If you get access to the channel without authorization, probably your HTTP backend is unavailable. You can check it with `curl` command. Open console on your server with Astra. And try to send request to the HTTP backend manually:

```
curl -v "https://auth.example.com/check"
```

Of course address should be same as in your settings.

If you see something like `Connection refused` or connection is stuck without any response, then issue with access to the backend.

### No access
