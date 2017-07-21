# Logging

Follow the Twelve-Factor App for more details on [logging](https://12factor.net/logs).



## Lessons learn from work

- Logging should be planned early - making a change can be a pain.
- Standardize log formats - preferably json rather than string. Not having a proper log standards will lead to inconsistent naming in your applications (especially microservices).

```javascript
// Bad
log('Success')
log('Ok')

// Good
log({
  event_type: 'SUCCESS',
  service_name: 'APP',
  version: '0.0.1',
  timestamp: Date.now() // Only when necessary
})
```
- Logs can be used for analytic purposes - the 5Ws and 1H applies here.
- Document your log practices. 
