# IV Expose model as Rest API

![bloks](../../static/bloks_dependencies.png)

```bash
make run-dev
curl http://0.0.0.0:8080/api/v1/rooms?filter[address.city][ilike]=paris&order_by[desc]=capacity
```