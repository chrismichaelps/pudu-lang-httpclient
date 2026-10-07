# Security policy

## Reporting a vulnerability

Report a suspected vulnerability privately through GitHub's
[security advisory form](https://github.com/chrismichaelps/pudu-lang-httpclient/security/advisories/new),
or by email to <chrisperezsantiago1@gmail.com> with `SECURITY` in the subject.

Please do not open a public issue for a vulnerability. Include the package version, the `pudu`
version, the platform, and the smallest program that shows the problem.

You can expect an acknowledgement within seven days and a decision on whether the report is
accepted within thirty.

## What is in scope

The package sends caller requests to the destinations they name and reads what comes back. A report
is in scope when it sends, keeps, or reads more than the caller allowed:

- Credentials (`authorization`, `cookie`, `proxy-authorization`) sent to an origin the caller did not
  name, or a request followed from https to http.
- A request reaching a host a `PublicOnly` address policy refuses, including through a redirect.
- A header value with a line break written to the wire, or a request line an address could split.
- A response head or body read past its limit, or a compressed body expanded past it.
- A response read from a connection as though it belonged to another request.
- A cookie sent to a host, path, or scheme it does not apply to.
- A sensitive header value written to an observer or a log.
- A connection, thread, or pipeline kept after the factory closed it, or unbounded growth of memory
  under concurrent use of one client.

## What is not in scope

- A handler that ignores its cancellation token. Cancellation is cooperative and bounded by the
  request's deadline.
- Requests to private addresses under the default `AnyAddress` policy; use `PublicOnly` where
  destinations come from users.
- Vulnerabilities in the Pudu compiler or standard library; report those to
  [pudu-lang](https://github.com/chrismichaelps/pudu-lang/security), and those of a dependency to
  that package's repository.

## Supported versions

| Version | Supported |
| ------- | --------- |
| 0.1.x   | Yes       |
