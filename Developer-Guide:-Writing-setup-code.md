Custom setup code allows for programmatic configuration of a clean installation including the provisioning of `Agents`, `Assets`, `users`, `rules`, etc. This means that the system loads in a pre-configured state that can easily be reproduced in a new instance.

Setup code is executed via the `SetupService` and is only executed if the `SETUP_WIPE_CLEAN_INSTALL` environment variable is set to `true` or if the database that the instance uses is empty. `SetupTasks` implementations are discovered using the standard java `ServiceLoader` mechanism and must therefore be registered via `META-INF/services` mechanism, generally implementations should extend the `EmptySetupTasks` which will do basic configuration of `keycloak`:

```
public class TestSetupTasks extends EmptySetupTasks {

    @Override
    public List<Setup> createTasks(Container container) {
        super.createTasks(container);

        // Do custom setup here
    }
```