# אלה הנקודות העיקריות שכדאי להכיר:

## 1. Server State vs Client State

הדבר הראשון והחשוב ביותר להבין.

### Client State

מידע שקיים רק באפליקציה.

לדוגמה:

* האם הדיאלוג פתוח
* ערך של input
* Theme
* Sidebar פתוח

בדרך כלל מנוהל עם:

* useState
* Context
* Zustand
* Jotai

---

### Server State

מידע שמגיע מהשרת.

לדוגמה:

```text
GET /users
GET /products
GET /orders
```

למידע הזה יש מאפיינים מיוחדים:

* לוקח זמן לקבל אותו
* הוא יכול להשתנות בשרת
* צריך Cache
* צריך Retry
* צריך Refetch
* יכול להיות לא עדכני

TanStack Query נועדה בדיוק לזה.

---

## 2. Query

Query = קריאה לשרת.

לדוגמה

```tsx
const users = useQuery({
  queryKey: ["users"],
  queryFn: getUsers,
});
```

אפשר לחשוב על Query כעל:

> "איך מביאים את הנתונים"

---

## 3. Query Key

זה אולי המושג הכי חשוב בספרייה.

```tsx
["users"]
```

או

```tsx
["users", userId]
```

או

```tsx
["products", category]
```

ה-Query Key הוא:

* מזהה ייחודי
* Cache Key
* הדרך של הספרייה לדעת האם כבר יש נתונים

לדוגמה

```tsx
["user", 5]
```

ו-

```tsx
["user", 6]
```

אלו שני Cache שונים לחלוטין.

שים לב אם תשתמש ב-queryKey של query אחר בטעות אתן לך נתונים לא נכונים.

---

## 4. Query Function

זו הפונקציה שמביאה את הנתונים.

```tsx
async function getUsers() {
    const res = await fetch("/api/users");
    return res.json();
}
```

ואז

```tsx
useQuery({
    queryKey: ["users"],
    queryFn: getUsers,
})
```

---

## 5. Cache

אחת הסיבות העיקריות להשתמש בספרייה.

בלי TanStack Query:

```
Page A

fetch users

↓

Page B

fetch users

↓

Page A

fetch users שוב
```

עם TanStack Query:

```
Page A

↓

fetch

↓

Cache

↓

Page B

↓

Cache

↓

Page A

↓

Cache
```

רוב הזמן אין Fetch נוסף.

---

## 6. Stale vs Fresh

זה אחד הרעיונות הכי חשובים.

הנתונים יכולים להיות:

```
Fresh
```

או

```
Stale
```

כברירת מחדל:

```text
staleTime = 0
```

כלומר:

ברגע שהבקשה הסתיימה

↓

הנתונים כבר נחשבים Stale.

אבל הם עדיין נשמרים ב-Cache.

אפשר לשנות:

```tsx
useQuery({
    queryKey: ["users"],
    queryFn: getUsers,
    staleTime: 1000 * 60 * 5,
})
```

עכשיו הם Fresh ל-5 דקות.

---

## 7. Background Refetch

אם הנתונים Stale:

TanStack Query יכולה לעדכן אותם ברקע.

לדוגמה:

```
Page

↓

Data from cache

↓

background fetch

↓

update UI
```

המשתמש רואה נתונים מיד.

---

## 8. Loading States

הספרייה מנהלת עבורך מצבים שונים.

```tsx
const {
    data,
    isLoading,
    isFetching,
    isError,
    error,
} = useQuery(...)
```

חשוב להבין את ההבדל:

### isLoading

אין עדיין Data.

פעם ראשונה.

---

### isFetching

כרגע מתבצעת בקשה.

יכול להיות שכבר יש Data.

לדוגמה:

```
Users

↓

show users

↓

background refetch

↓

isFetching = true
```

אבל הנתונים עדיין מוצגים.

---

## 9. Error Handling

```tsx
if (isError)
    return <Error />;
```

הספרייה גם יודעת לבצע Retry אוטומטי.

ברירת מחדל:

```text
3 retries
```

---

## 10. Mutations

Queries קוראות מידע.

Mutations משנות מידע.

לדוגמה:

```text
POST

PUT

PATCH

DELETE
```

```tsx
const mutation = useMutation({
    mutationFn: createUser,
});
```

---

## 11. Invalidation

אחרי Mutation צריך לעדכן את ה-Cache.

לדוגמה

```
POST /users
```

אחר כך:

```tsx
queryClient.invalidateQueries({
    queryKey: ["users"],
});
```

המשמעות:

"הנתונים כבר לא עדכניים."

TanStack Query תבצע Fetch חדש בפעם הבאה שה-Query תהיה פעילה (ולעתים מיד אם יש לה מנויים).

---

## 12. Optimistic Updates

אפשר לעדכן את ה-UI לפני שהשרת החזיר תשובה.

לדוגמה

```
Click Like

↓

UI מתעדכן

↓

Request

↓

Success

↓

Done
```

אם יש שגיאה:

```
Rollback
```

זה נותן תחושה מהירה מאוד.

---

## 13. Prefetch

אפשר להביא מידע מראש.

לדוגמה:

המשתמש עומד לעבור לעמוד הבא.

```tsx
queryClient.prefetchQuery(...)
```

כשהוא יגיע:

המידע כבר ב-Cache.

---

## 14. Infinite Queries

לטעינה אינסופית.

לדוגמה:

```
Instagram

Facebook

Twitter

TikTok
```

```tsx
useInfiniteQuery(...)
```

---

## 15. Pagination

לדוגמה

```
?page=1

?page=2

?page=3
```

כל עמוד נשמר ב-Cache.

---

## 16. Dependent Queries

Query אחת תלויה באחרת.

```tsx
const user = useQuery(...)

const posts = useQuery({
    enabled: !!user.data,
})
```

לא תתבצע עד שהראשונה הסתיימה.

---

## 17. QueryClient

זה ה"מוח" של הספרייה.

הוא מחזיק:

* Cache
* Queries
* Mutations
* Invalidation
* Prefetch

לכן עוטפים את האפליקציה:

```tsx
<QueryClientProvider client={queryClient}>
```

---

## 18. Devtools

מומלץ כמעט בכל פרויקט.

אפשר לראות:

* Cache
* Query Keys
* Loading
* Refetches
* Stale/Fresh
* Observers

---

## 19. Select

לא חייבים להשתמש בכל הנתונים.

```tsx
const users = useQuery({
    queryKey: ["users"],
    queryFn: getUsers,
    select: data => data.users,
})
```

מעולה כשאתה רוצה שהקומפוננטה תראה רק את החלק שמעניין אותה.

---

## 20. Enabled

לא תמיד רוצים לבצע Query מיד.

```tsx
useQuery({
    queryKey: ["user", id],
    queryFn: getUser,
    enabled: !!id,
})
```

רק כשיש `id`.

---

### מה חשוב במיוחד למפתח React

אם אתה כבר עובד עם React, הייתי מוודא שאתה מבין היטב את הנושאים האלה, כי הם מכסים את רוב השימוש היומיומי:

1. ההבדל בין **Server State** ל-**Client State**.
2. איך `useQuery` עובד.
3. למה `queryKey` חייב להיות ייחודי ויציב.
4. איך פועל ה-Cache.
5. מה ההבדל בין **Fresh** ל-**Stale** (`staleTime`).
6. ההבדל בין `isLoading` ל-`isFetching`.
7. איך משתמשים ב-`useMutation`.
8. למה צריך `invalidateQueries` אחרי שינוי נתונים.
9. איך מגדירים Queries תלויות (`enabled`).
10. איך להשתמש ב-Devtools כדי להבין מה קורה מאחורי הקלעים.

מכיוון שכבר שאלת בעבר על **TanStack Form**, תראה שהדפוס דומה: הספרייה מספקת API שמנהל עבורך את המצב המורכב (State Machine + Cache), ואתה מתאר *מה* אתה רוצה (שאילתה, מפתח, Mutation) במקום לנהל ידנית `useEffect`, `loading`, `error`, `retry` ו־`cache`.
