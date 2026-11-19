# Local review evidence for GoGaucho PR #78

Tested [draft PR #78](https://github.com/GoGaucho/web/pull/78) at commit `b81da902c065065e7a86b062451717069ad6ded1` on November 19, 2026. The browser test used the draft's Vue components, date snapshot, recurrence serializer and Calendar client. Firebase, Google's authorization result and network writes were replaced by an isolated fixture and request recorder. The test used synthetic accounts and schedule data.

## Browser walkthrough

1. [My Classes](./01-my-classes.jpg) shows a Fall 2028 Friday class and a custom Friday event. Once both schedule records are available, **Add to Google Calendar** is enabled.
2. [Export dialog](./02-export-dialog.jpg) confirms the quarter and describes selecting a personal or school Google account. During this test, the fixture returned authorization for a synthetic personal account.
3. [Recorder result](./03-verified-export.jpg) shows five passing assertions. [The captured requests](./calendar-requests.json) show one calendar creation and two event inserts. Both events use Pacific time and private visibility. The enrolled event excludes November 10 and 24, 2028, while the custom event repeats normally. The recorder checked the generated destination link.
4. [Permission denied](./04-permission-denied.jpg) shows the cancellation message. [The separate denied run](./denial-result.json) recorded zero Calendar writes.

## Tests and holiday scope

- [Test output](./tests.log.txt) shows **154 passed, 0 failed** from `npm test`.
- [Production build output](./build.log.txt) shows a successful `npm run build`.
- [Fall 2026 recurrence check](./holiday-scope.json) confirms that a Monday class remains scheduled on October 12, a federal holiday outside UCSB’s published no-class list. The same check confirms that enrolled Wednesday and Friday meetings skip Veterans Day and Thanksgiving Friday, and a custom Friday event retains its recurrence. [UCSB Campus Calendar](https://catalog.ucsb.edu/academicsandpolicies/introcampus/calendar), [Federal holiday calendar](https://www.opm.gov/policy-data-oversight/pay-leave/federal-holidays/)

The final holiday-aware build still needs a live Google Calendar write as a release check. These local requests did not go to Google or modify any Google account.
