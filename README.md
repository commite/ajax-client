# Ajax Client

Observable based HTTP client on top of [rxjs/ajax](https://rxjs-dev.firebaseapp.com/api/ajax/ajax) for browser and node.js.

## Table of contents

- [Ajax Client](#ajax-client)
  - [Table of contents](#table-of-contents)
  - [Installation](#installation)
  - [Dependencies](#dependencies)
  - [API](#api)
    - [Get/Delete](#getdelete)
      - [Get example](#get-example)
    - [Post/Put/Patch](#postputpatch)
      - [Post example](#post-example)
    - [Request](#request)
      - [Request example](#request-example)
    - [Interceptors](#interceptors)
      - [Request interceptor](#request-interceptor)
      - [Response interceptor](#response-interceptor)
  - [TypeScript](#typescript)
  - [Building/Testing](#buildingtesting)
  - [Contributing](#contributing)
  - [License](#license)

## Installation

```bash
npm i --save @commite/ajax-client rxjs
```

## Dependencies

```json
"rxjs": "^6.3.3"
```

## API

**Ajax client** uses [rxjs/ajax](https://rxjs-dev.firebaseapp.com/api/ajax/ajax) **API** to perform calls and returns [rxjs/observable](https://rxjs-dev.firebaseapp.com/guide/observable).

All request accepts an [Ajax request options](https://rxjs-dev.firebaseapp.com/api/ajax/AjaxRequest), `post`, `put` and `patch` requests body param can also be typed, other way `any` type will be assumed.

All request returns an [Ajax response](https://rxjs-dev.firebaseapp.com/api/ajax/AjaxResponse), `response` param can also be typed, other way `any` type will be assumed.

### Get/Delete

```ts
[get|delete]<T>(url: string, options?: Partial<AjaxClientRequest<null>>): Observable<AjaxClientResponse<T>>
```

#### Get example

```ts
const ajaxClient = new AjaxClient();

ajaxClient.get<GetUsersResponse>('/users', { headers: ... }).subscribe((userRes) => {
  [...]
});
```

[Back to top](#table-of-contents)

### Post/Put/Patch

```ts
[post|put|patch]<T, Y>(url: string, body: Y, options?: Partial<AjaxClientRequest<Y>): Observable<AjaxClientResponse<T>>
```

#### Post example

```ts
const ajaxClient = new AjaxClient();

ajaxClient.post<PostUserResponse, PostUserBody>('/users', { email: 'test@test.com'}, { headers: ... }).subscribe((userRes) => {
  [...]
});

```

[Back to top](#table-of-contents)

### Request

```ts
request<T, Y>(options: Partial<AjaxClientRequest<Y>>): Observable<AjaxClientResponse<T>>
```

#### Request example

```ts
ajaxClient.request<RequestResponse, RequestBody>({
  method: 'POST',
  body: {...},
  headers: {...}
}).subscribe((reqRes) => {
  [...]
})
```

[Back to top](#table-of-contents)

### Interceptors

Request and response could be intercepted.

#### Request interceptor

A request interceptor is a function that accepts a `Partial<AjaxClientRequest<T>>` object and returns a modified (or not) `Partial<AjaxClientRequest<T>>`.

```ts
RequestInterceptor = <T>(
  options: Partial<AjaxRequest<T>>
) => Partial<AjaxRequest<T>>
```

Request interceptors can be managed through `ajaxClient.interceptors.request` array.

```ts
ajaxClient.interceptors.request.push(options => {
  return {
    ...options,
    url: `https://my-base-url.com/${options.url}`
  };
});
```

#### Response interceptor

A response interceptor is a function that accepts a `AjaxClientResponse<T>` object and returns a modified (or not) `AjaxClientResponse<T>`.

```ts
ResponseInterceptor = <T>(response: AjaxClientResponse<T>) => AjaxClientResponse<T>;
```

Response interceptors can be managed through `ajaxClient.interceptors.response` array.

```ts
ajaxClient.interceptors.response.push(ajaxResponse => {
  return {
    ...ajaxResponse,
    response: ajaxResponse.response.split(',')
  };
});
```

[Back to top](#table-of-contents)

## TypeScript

Ajax client is coded entirely in TypeScript with `target:'es5'`.

## Building/Testing

- `npm install`
- `npm run build`
- `npm run lint`
- `npm run test` or `npm run test:watch`

## Contributing

- Maintaining 100% unit tests coverage and lintering.
- [Gitflow](https://es.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
- [Commit convention](https://karma-runner.github.io/3.0/dev/git-commit-msg.html)
- [Semver](https://semver.org/spec/v2.0.0.html)

## License

Copyright (c) 2018 **[Commite Inc](https://commite.co/)** under **[LGPLv3](https://choosealicense.com/licenses/lgpl-3.0/)** license.
