# nova-mysql

MySQL/MariaDB driver for Nova on the async runtime. A Nova package — fetch with:

```sh
nova get https://github.com/kamlesh-nb/nova-mysql
```

```nova
import mysql;
```

## Structure (SOLID)

Split by responsibility; consumers only touch the seam (`MyDriver` / `MyConnection`).

| Module       | Responsibility |
|--------------|----------------|
| `mysql`      | Seam: `MyConnection impl Connection` + `MyDriver impl Driver` + connect/handshake. |
| `connection`     | Connection-string parsing (`ConnectionOptions`, `parse`). |
| `codec`   | Wire codec — packet framing, builders, decoders (incl. binary protocol), `MyCursor`. |
| `typemap` | MySQL type→`DbType`, `DbValue`→SQL/bind text, `substituteParams`. |
| `proto`   | Async transport framing (`MyReader`, `Packet`, `readPacket`, `sendPacket`). |
| `stmt`    | Prepared-statement cache entry (`MyStmt`). |
| `auth`    | Auth scrambles (native / sha2 / caching_sha2 / RSA) + capability flags. |
