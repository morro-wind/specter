haproxy sni check proxy configuration

## example.liepass.com
backend	example
	mode http
	balance	random

	option	abortonclose
	option forwardfor header Client-IP

	cookie JSESSIONID prefix

	option	httpchk
	#http-check send meth GET uri / ver HTTP/1.1 hdr host https://example.liepass.com
        http-check send meth GET uri /v1/getPayInfo ver HTTP/1.1 hdr Host example.liepass.com

	#http-send-name-header Host
        http-request set-header Host example.liepass.com
        http-request set-header X-Forwarded-Proto https
	server	example.liepass.com  example.liepass.com:443  ssl  verify  none  sni  str(example.liepass.com) resolvers mydns