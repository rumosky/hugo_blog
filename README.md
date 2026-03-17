$env:HTTP_PROXY="http://127.0.0.1:1080"
$env:HTTPS_PROXY="http://127.0.0.1:1080"

git config --global http.proxy http://127.0.0.1:1080
git config --global https.proxy http://127.0.0.1:1080


git config --global --unset http.proxy