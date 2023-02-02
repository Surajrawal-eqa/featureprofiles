# The `addrundata` Tool

The `addrundata` tool keeps all the `rundata_test.go` files up to date in the
featureprofiles repo. These files contain the rundata that identify each test,
and the rundata will find its way into the test XML output when a functional
test is run with the `-xml` flag. The rundata allow us to track the test result.

There are two modes of operation:

*   Check mode: `go run ./tools/addrundata`

    When run without a flag, this checks the integrity of rundata in the repo.
    This is used by the "[Rundata Check]" pull request check. If the check
    fails, we will not be able to track the test result from those tests with
    outdated rundata.

*   Fix mode: `go run ./tools/addrundata --fix`

    This will update any outdated rundata, to be run by the author of a pull
    request if the "[Rundata Check]" fails.

[Rundata Check]: /.github/workflows/rundata_check.yml

An example `rundata_test.go` looks like this:

```go
// Code generated by go run tools/addrundata; DO NOT EDIT.
package foo_test

import "github.com/openconfig/featureprofiles/internal/rundata"

func init() {
    rundata.TestPlanID = "XX-1.1"
    rundata.TestDescription = "Foo Functional Test"
    rundata.TestUUID = "bf60afdc-7130-4bef-a23c-39783c7f2bb3"
}
```

Both `TestPlanID` and `TestDescription` are sourced from the top-level heading
in `README.md`:

```md
# XX-1.1: Foo Functional Test

## Summary

One line summary of what foo functional test does.
```

But the `TestUUID` is uniquely generated for each test. The `addrundata` tool
takes care of the UUID generation. Both the `ate_tests` and `otg_tests` variants
of the same test must have the same rundata.