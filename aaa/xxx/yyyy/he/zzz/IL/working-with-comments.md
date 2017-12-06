---
כותרת: עבודה עם תגובות
---


{:toc}

לכל בקשה Pull, {{ site.data.variables.product.product_name }} מספקת שלושה סוגים של תצוגות תגובה:
[comments on the Pull Request][PR comment] בכללותו, [comments on a specific line][PR line comment] בתוך בקשת Pull,
ו [comments on a specific commit][commit comment] בתוך הבקשה למשוך.

כל הסוגים הללו של הערות עובר חלק אחר של API {{ site.data.variables.product.product_name }}.
במדריך זה, אנו נחקור איך אתה יכול לגשת ולטפל בכל אחד. לכל
למשל, אנחנו נשתמש [this sample Pull Request made][sample PR] על "octocat"
מאגר. כמו תמיד, ניתן למצוא דגימות ב [our platform-samples repository][platform-samples].

## תגובות בקשת Pull

כדי לגשת הערות על בקשת Pull, תצטרך לעבור [the Issues API][issues].
זה אולי נראה מנוגד בהתחלה. אבל ברגע שאתה מבין כי Pull
בקשה אינה רק בעיה עם קוד, זה הגיוני להשתמש ב- API התכנים
ליצור הערות על בקשה למשוך.

נצטרך להפגין מקסים הערות בקשה משוך על ידי יצירת סקריפט רובי באמצעות
[Octokit.rb][octokit.rb]. מומלץ גם ליצור [personal access token][personal token].

הקוד הבא אמור לעזור לך להתחיל בגישת הערות מבקשה Pull
באמצעות Octokit.rb:

`` `אודם
דורש "octokit"

# !!! אין להשתמש EVER ערכים בקידוד קשיח ב- App אמיתי !!!
# במקום, להגדיר סביבת הבדיקה משתנים, כמו בדוגמא הבאה
לקוח = Octokit :: Client.new: ACCESS_TOKEN => ENV['MY_PERSONAL_TOKEN']

client.issue_comments ( "octocat / Spoon-סכין", 1176) .each לעשות | תגובה |
שם המשתמש = comment[:user][:login]
post_date = comment[:created_at]
תוכן = comment[:body]

מכניס "# {username} העיר הערה על # {post_date} זה אומר: \n '# {content}'\n"
סוף
```

הנה, אנחנו קוראים את ספציפי ל- API הסוגיות כדי לקבל את ההערות ( `issue_comments`),
מתן גם את השם של המאגר ( `octocat / Spoon-Knife`), ואת זהות בקשת Pull
אנחנו מתעניינים ( `1176`). אחרי זה, זה פשוט עניין של ולביקורות דרך
ההערות להביא מידע על כל אחד.

## תגובות בקשה משוך על קו

מתוך תצוגת ההבדל, אתה יכול להתחיל דיון על היבט מסוים של יחיד
לשנות שנעשה בתוך הבקשה למשוך. הערות אלה מתרחשות על הקווים הפרט
בתוך קובץ שונה. כתובת האתר Endpoint של הדיון הזה מגיע [the Pull Request Review API][PR Review API].

הקוד הבא מביא את כל הערות בקשת Pull שנעשו על קבצים, קבל מספר בקשת Pull יחיד:

`` `אודם
דורש "octokit"

# !!! אין להשתמש EVER ערכים בקידוד קשיח ב- App אמיתי !!!
# במקום, להגדיר סביבת הבדיקה משתנים, כמו בדוגמא הבאה
לקוח = Octokit :: Client.new: ACCESS_TOKEN => ENV['MY_PERSONAL_TOKEN']

client.pull_request_comments ( "octocat / Spoon-סכין", 1176) .each לעשות | תגובה |
שם המשתמש = comment[:user][:login]
post_date = comment[:created_at]
תוכן = comment[:body]
נתיב = comment[:path]
עמדה = comment[:position]

מכניס "# {username} העיר הערה על # {post_date} עבור קובץ שנקרא # {path}, על קו # {position} זה אומר: \n '# {content}'\n"
סוף
```

תוכל להבחין שזה מאוד דומה בדוגמה לעיל. ההבדל
בין השקפה זו לבין תגובת בקשת Pull הוא מוקד השיחה.
הוסיף הערה על בקשת Pull צריכה להיות שמורה דיון או רעיונות
הכיוון הכללי של הקוד. לתגובה שפורסמה כחלק מבקש סקירת Pull צריך
לטפל ספציפית דרך שינוי מסוים יושם בתוך קובץ.

## להתחייב תגובות

הסוג האחרון של הערות להתרחש דווקא על תתחייב פרט. מהסיבה הזו,
הם עושים שימוש [the commit comment API][commit comment API].

כדי לאחזר את ההערות על להתחייב, אתה תרצה להשתמש SHA1 של להתחייב.
במילים אחרות, לא תוכל להשתמש בכל מזהה המתייחסים לבקשה למשוך. הנה דוגמה:

`` `אודם
דורש "octokit"

# !!! אין להשתמש EVER ערכים בקידוד קשיח ב- App אמיתי !!!
# במקום, להגדיר סביבת הבדיקה משתנים, כמו בדוגמא הבאה
לקוח = Octokit :: Client.new: ACCESS_TOKEN => ENV['MY_PERSONAL_TOKEN']

client.commit_comments ( "octocat / Spoon-סכין", "cbc28e7c8caee26febc8c013b0adfb97a4edd96e") כל לעשות |. תגובה |
שם המשתמש = comment[:user][:login]
post_date = comment[:created_at]
תוכן = comment[:body]

מכניס "# {username} העיר הערה על # {post_date} זה אומר: \n '# {content}'\n"
סוף
```

שים לב API שיחה זו תהיה לאחזר תגובות שורה אחת, כמו גם דבריו
על כל להתחייב.

[PR comment]: https://github.com/octocat/Spoon-Knife/pull/1176#issuecomment-24114792
[PR line comment]: https://github.com/octocat/Spoon-Knife/pull/1176#discussion_r6252889
[commit comment]: https://github.com/octocat/Spoon-Knife/commit/cbc28e7c8caee26febc8c013b0adfb97a4edd96e#commitcomment-4049848
[sample PR]: https://github.com/octocat/Spoon-Knife/pull/1176
[platform-samples]: https://github.com/github/platform-samples/tree/master/api/ruby/working-with-comments
[issues]: https://developer.github.com/v3/issues/comments/
[personal token]: https://help.github.com/articles/creating-an-access-token-for-command-line-use
[octokit.rb]: https://github.com/octokit/octokit.rb
[PR Review API]: https://developer.github.com/v3/pulls/comments/
[commit comment API]: https://developer.github.com/v3/repos/comments/#get-a-single-commit-comment
