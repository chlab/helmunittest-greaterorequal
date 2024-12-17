# Helm Unittest Bug Reproduction

This repo is a minimal reproduction of a bug with the `greaterOrEqual` comparison of [helm unittest](https://github.com/helm-unittest/helm-unittest).

To reproduce, run `helm unittest . -f tests/test-pdb.yaml`. You should see the following error:

```
### Chart [ greaterOrEqualTest ] .

 FAIL  PodDisruptionBudget	tests/test-pdb.yaml
	- Deployment should have a proper configuration of `PodDisruptionBudget`

		- asserts[1] `greaterOrEqual` fail
			Template:	greaterOrEqualTest/templates/pdb.yaml
			DocumentIndex:	0
			ValuesIndex:	0
			Path:	spec.minAvailable
			Expected to be greater then or equal to, got:
				the expected '1' is not greater or equal to the actual '2'
```

The comparison seems to be testing the test value as the expectation, instead of the other way around. 
The test should pass because `minAvailable` is set to 2 and we expect `greaterOrEqual` to 1.

Tested with version 0.7.0