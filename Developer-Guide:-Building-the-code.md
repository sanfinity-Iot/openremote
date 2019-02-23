Ensure you have prepared your environment as described [here](./Developer-Guide%3A-Preparing-the-environment).

Building the code is just a matter of executing the following gradle tasks:

```
./gradlew clean installDist
```

If you want to run the tests (requires running background service containers on the host), execute:

```
./gradlew clean build installDist
```

When background services are not available, the test harness will try to connect for a long time before giving up. TODO: Fix this, fail fast when test harness can't access services.

# See Also
- [Get Started](https://openremote.io/get-started-manager/)