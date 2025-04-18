## UDPWAF
Listens for UDP taffic and forwards it to specific host.</br>
Acts like proxy with additional optional WAF.</br>
Calls python script with data and if it returns bytes object it will forward that data,</br>
if it gets None packet will be dropped.

### Building
Requires ```python3-dev``` on debian/ubuntu.</br>
run ```cargo build -r```
### Remarks

Currently script is open at aplication start.</br>
You can request reload of python script by sending SIGUSR1 signal.</br>
It will also remove all active clients.</br>
Added supprt to change number of descriptors open limits.</br>
Under stress testing default 1024 is hit very fast.</br>
If you get error for too many open files open change hard limit to more than 30k</br>
Clients are dropped if there is no activity for number of seconds, set in timeout.</br>
</br>
Logger can be configured for trace messages</br>
run ```RUST_LOG=trace cargo run -r```</br>
</br>
Added experimental [landlock](https://landlock.io/). Restrics filesystem usage to current working dir and no TCP.</br>
Requires kernel 6.10 or later to fully support all ABI restrictions.</br>
Assumes python is installed in /usr/bin and /usr/lib</br>
You can get ```Forwarder: Error calling python: PermissionError: [Errno 13] Permission denied``` from python or other errors.
### Usage:

```
Usage: udpwaf [OPTIONS]

Options:
  -b, --bind <BIND>                    Server bind address [default: [::]:8080]
  -f, --forward <FORWARD>              Where to forward messages [default: [::1]:8000]
  -l, --local-bind <LOCAL_BIND>        Where to listent to for forwarder [default: [::1]:0]
  -t, --timeout <TIMEOUT>              After this time client connection will be dropped [default: 10]
  -s, --filter-script <FILTER_SCRIPT>  Python script should contain filter function. [default: script.py]
  -h, --help                           Print help
  -V, --version                        Print version
