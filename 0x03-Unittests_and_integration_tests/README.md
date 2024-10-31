# Unittests and Integration Tests

This project contains tasks for learning to write unittests and integration tests in Python 3.

## Required Modules

- `parameterized`

## Tasks To Complete

### 0. Parameterize a unit test

The `test_utils.py` module contains a Python module that meets the following requirements:

- Familiarize yourself with the `utils.access_nested_map` function and understand its purpose. Play with it in the Python console to make sure you understand.
- Write the first unit test for `utils.access_nested_map`.
- Create a `TestAccessNestedMap` class that inherits from `unittest.TestCase`.
- Implement the `TestAccessNestedMap.test_access_nested_map` method to test that the method returns what it is supposed to.
- Decorate the method with `@parameterized.expand` to test the function for the following inputs:

  ```python
  nested_map={"a": 1}, path=("a",)
  nested_map={"a": {"b": 2}}, path=("a",)
  nested_map={"a": {"b": 2}}, path=("a", "b")
  ```

- For each of these inputs, test with `assertEqual` that the function returns the expected result.
- The body of the test method should not be longer than 2 lines.

### 1. Parameterize a unit test

The `test_utils.py` module contains a Python module that meets the following requirements:

- Implement `TestAccessNestedMap.test_access_nested_map_exception`. Use the `assertRaises` context manager to test that a `KeyError` is raised for the following inputs (use `@parameterized.expand`):

  ```python
  nested_map={}, path=("a",)
  nested_map={"a": 1}, path=("a", "b")
  ```

### 2. Mock HTTP calls

The `test_utils.py` module contains a Python module that meets the following requirements:

- Familiarize yourself with the `utils.get_json` function.
- Define the `TestGetJson(unittest.TestCase)` class and implement the `TestGetJson.test_get_json` method to test that `utils.get_json` returns the expected result.
- Use `unittest.mock.patch` to patch `requests.get`. Make sure it returns a `Mock` object with a `json` method that returns `test_payload` which you parametrize alongside the `test_url` that you will pass to `get_json` with the following inputs:

  ```python
  test_url="http://example.com", test_payload={"payload": True}
  test_url="http://holberton.io", test_payload={"payload": False}
  ```

### 3. Parameterize and patch

The `test_utils.py` module contains a Python module that meets the following requirements:

- Test that the output of `get_json` is equal to `test_payload`.
- Read about memoization and familiarize yourself with the `utils.memoize` decorator.
- Implement the `TestMemoize(unittest.TestCase)` class with a `test_memoize` method.
- Inside `test_memoize`, define the following class:

  ```python
  class TestClass:
      def a_method(self):
          return 42

      @memoize
      def a_property(self):
          return self.a_method()
  ```

- Use `unittest.mock.patch` to mock `a_method`. Test that when calling `a_property` twice, the correct result is returned but `a_method` is only called once using `assert_called_once`.

### 4. Parameterize and patch as decorators

The `test_client.py` module contains a Python module that meets the following requirements:

- Familiarize yourself with the `client.GithubOrgClient` class.
- Declare the `TestGithubOrgClient(unittest.TestCase)` class and implement the `test_org` method.
- This method should test that `GithubOrgClient.org` returns the correct value.
- Use `@patch` as a decorator to make sure `get_json` is called once with the expected argument but make sure it is not executed.

### 5. Mocking a property

The `test_client.py` module contains a Python module that meets the following requirements:

- `google`
- `abc`
- No external HTTP calls should be made.

### 6. More patching

The `test_client.py` module contains a Python module that meets the following requirements:

- `memoize` turns methods into properties. Read up on how to mock a property.
- Implement the `test_public_repos_url` method to unit-test `GithubOrgClient._public_repos_url`.
- Use `patch` as a context manager to patch `GithubOrgClient.org` and make it return a known payload.
- Test that the result of `_public_repos_url` is the expected one based on the mocked payload.

### 7. Parameterize

The `test_client.py` module contains a Python module that meets the following requirements:

- Implement `TestGithubOrgClient.test_public_repos` to unit-test `GithubOrgClient.public_repos`.
- Use `@patch` as a decorator to mock `get_json` and make it return a payload of your choice.
- Use `patch` as a context manager to mock `GithubOrgClient._public_repos_url` and return a value of your choice.
- Test that the list of repos is what you expect from the chosen payload.
- Test that the mocked property and the mocked `get_json` was called once.

### 8. Integration test: fixtures

The `test_client.py` module contains a Python module that meets the following requirements:

- We want to test the `GithubOrgClient.public_repos` method in an integration test. That means that we will only mock code that sends external requests.
- Create the `TestIntegrationGithubOrgClient(unittest.TestCase)` class and implement the `setUpClass` and `tearDownClass` which are part of the `unittest.TestCase` API.
- Use `@parameterized_class` to decorate the class and parameterize it with fixtures found in `fixtures.py`. The file contains the following fixtures:

  ```python
  org_payload, repos_payload, expected_repos, apache2_repos
  ```

- The `setupClass` should mock `requests.get` to return example payloads found in the fixtures.
- Use `patch` to start a patcher named `get_patcher`, and use `side_effect` to make sure the mock of `requests.get(url).json()` returns the correct fixtures for the various values of `url` that you anticipate to receive.
- Implement the `tearDownClass` class method to stop the patcher.

### 9. Integration tests

The `test_client.py` module contains a Python module that meets the following requirements:

- Implement the `test_public_repos` method to test `GithubOrgClient.public_repos`.
- Make sure that the method returns the expected results based on the fixtures.
- Implement `test_public_repos_with_license` to test the `public_repos` with the argument `license="apache-2.0"` and make sure the result matches the expected value from the fixtures.
