## Questions Index

* [Ques 1](#ques-1-show-the-the-percentage-who-strongly-agree)

* [Ques 2](#ques-2-show-the-institution-and-subject-where-the-score-is-at-least-100-for-question-15)

* [Ques 3](#ques-3-show-the-institution-and-score-where-the-score-for-8-computer-science-is-less-than-50-for-question-q15)

* [Ques 4](#ques-4-show-the-subject-and-total-number-of-students-who-responded-to-question-22-for-each-of-the-subjects-8-computer-science-and-h-creative-arts-and-design)

* [Ques 5](#ques-5-show-the-subject-and-total-number-of-students-who-a_strongly_agree-to-question-22-for-each-of-the-subjects-8-computer-science-and-h-creative-arts-and-design)

* [Ques 6](#ques-6-show-the-percentage-of-students-who-a_strongly_agree-to-question-22-for-the-subject-8-computer-science-show-the-same-figure-for-the-subject-h-creative-arts-and-design)

* [Ques 7](#ques-7-show-the-average-scores-for-question-q22-for-each-institution-that-include-manchester-in-the-name)

* [Ques 8](#ques-8-show-the-institution-the-total-sample-size-and-the-number-of-computing-students-for-institutions-in-manchester-for-q01)


### Ques 1. Show the the percentage who STRONGLY AGREE.

```sql
SELECT A_STRONGLY_AGREE
FROM nss
WHERE question='Q01'
AND institution='Edinburgh Napier University'
AND subject='(8) Computer Science'
```

### Ques 2. Show the institution and subject where the score is at least 100 for question 15.

```sql
SELECT institution, subject
FROM nss
WHERE question='Q15' AND score >= 100
```

### Ques 3. Show the institution and score where the score for '(8) Computer Science' is less than 50 for question 'Q15'.

```sql
SELECT institution, score
FROM nss
WHERE subject='(8) Computer Science'
  AND question='Q15'
  AND score < 50
```

### Ques 4. Show the subject and total number of students who responded to question 22 for each of the subjects '(8) Computer Science' and '(H) Creative Arts and Design'.

```sql
SELECT subject, SUM(response) AS total_students
FROM nss
WHERE subject IN ('(8) Computer Science', '(H) Creative Arts and Design')
  AND question='Q22'
GROUP BY subject
```

### Ques 5. Show the subject and total number of students who A_STRONGLY_AGREE to question 22 for each of the subjects '(8) Computer Science' and '(H) Creative Arts and Design'.

```sql
SELECT subject, SUM(response*A_STRONGLY_AGREE/100) AS total_students
FROM nss
WHERE subject IN ('(8) Computer Science', '(H) Creative Arts and Design')
  AND question='Q22'
GROUP BY subject
```


### Ques 6. Show the percentage of students who A_STRONGLY_AGREE to question 22 for the subject '(8) Computer Science' show the same figure for the subject '(H) Creative Arts and Design'.

```sql
SELECT subject,
       ROUND(SUM(response*A_STRONGLY_AGREE/100) * 100 / SUM(response), 1) AS agreed_students_percentage
FROM nss
WHERE subject IN ('(8) Computer Science', '(H) Creative Arts and Design')
  AND question='Q22'
GROUP BY subject
```

### Ques 7. Show the average scores for question 'Q22' for each institution that include 'Manchester' in the name.

```sql
SELECT institution,
       ROUND(SUM(score * response) / SUM(response), 1)
FROM nss
WHERE question='Q22'
  AND institution LIKE '%Manchester%'
GROUP BY institution
```

### Ques 8. Show the institution, the total sample size and the number of computing students for institutions in Manchester for 'Q01'.

```sql
SELECT institution,
       SUM(sample) AS total_sample,
       SUM(CASE WHEN subject='(8) Computer Science' THEN sample ELSE 0 END) AS computing_students
FROM nss
WHERE question='Q01'
  AND (institution LIKE '%Manchester%')
GROUP BY institution
```
