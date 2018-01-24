
כפי [announced earlier today][heartbleed-blog-post], אנו מגיבים באופן פעיל
לביטחון [Heartbleed-חשף לאחרונה
הפגיעות] [heartbleed-blog-post] ב OpenSSL. בעוד בשלב זה יש GitHub
אין אינדיקציה כי ההתקפה הייתה בשימוש מעבר לבחינת הפגיעות, אנחנו
ממליץ אינטגרטורים [reset the API authorizations][api] עבור OAuth שלהם
יישומים.

הוספנו [new API method][api] למטרה זו בדיוק. קורא בשיטה זו
יבטל את הקוד הישן ולהחזיר אסימון חדש עבור יישומים לאחסן
ולהשתמש במקומו. שיטה חדשה זו מספקת דרך בטוחה לאפס המשתמשים
אישורים ללא צורך משתמש לאשר אותו מחדש את היישום על
אינטרנט.

אינטגרטורים יכול גם להשתמש בשיטות ביטול קיימות ~~ לבטל את כל
אסימונים ~~ או [revoke a single token][] עבור היישומים שלהם.

{{#tip}}

** עדכון (2016/01/25): ** v3 API כבר לא מספק שיטה לבטל <em>all</em> של אסימונים של יישום כמו בעבר בסימוכין. אם אתה צריך לבטל את כל האסימונים עבור היישום שלך, אתה יכול לעשות זאת דרך דף <a href="https://github.com/settings/developers">settings עבור application</a> שלך.

{{/tip}}

אם יש לך שאלות או משוב, בבקשה! [get in touch] [contact]

[contact]: https://github.com/contact?form[subject]=API+resetting+tokens
[api]: / v3 / oauth_authorizations / # לאפס-an-אישור
[revoke a single token]: / v3 / oauth_authorizations / # לשלול-an-אישור-עבור-an-יישום
[heartbleed-blog-post]: https://github.com/blog/1818-security-heartbleed-vulnerability
