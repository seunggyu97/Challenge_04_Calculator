# Challenge_04
## 계산기

TableLayout을 이용하여 계산기의 형태로 UI를 구성한 뒤, 사용자의 입력에 따라 연산을 수행한다.

## 기능적 제약사항
1. 연산자는 1회만 사용하도록 한다. 

    (예를들어, '+' 연산을 사용한 뒤 바로 '*' 연산을 사용하지 못한다. 스택을 사용하면서 후위연산을 진행해야 하는데, 이 프로젝트가 끝난 뒤 따로 구현해 보는게 좋을것 같다.)

2. 피연산자로 들어가는 수는 최대 15자리 까지만 입력받을 수 있다.

## 학습 내용
* TableLayout
  *	Row
  *	Column
* ConstraitLayout
* LayoutInflater
  * LayoutInflater.from(context).inflate(레이아웃파일, root, attachToRoot)
  * `val historyView = LayoutInflater.from(this).inflate(R.layout.history_row, null, false)`
* Thread
  * Thread에는 Main Thread와 다른 Thread가 존재
  * Main Thread가 아닌 다른 Thread에서는 네트워크 작업이나 DB 작업과 같이 Main Thread에서 실행하기엔 무거운 작업들을 사용
  * Ex: `Thread(Runnable { … }).start()`
``` kotlin
Thread(Runnable {
    db.historyDao().getAll().reversed().forEach {

        runOnUiThread {

            val historyView =

                LayoutInflater.from(this).inflate(R.layout.history_row, null, false)

            historyView.findViewById<TextView>(R.id.expressionTextView).text = it.expression

            historyView.findViewById<TextView>(R.id.resultTextView).text = "= ${it.result}"

            historyLinearLayout.addView(historyView)

         }}}).start()
```
  *	Thread 작업 후 다시 UI에 그리기 위해서는 runOnUiThread 를 사용
* Room
  * Local DB로 생각하면 쉽다. (AppDatabase 파일, model, dao 패키지 참고)
  * 사용 방법
  1.	AppDatabase의 추상클래스를 새로 만들고 Database를 먼저 선언한다.
  2.	Dao에 해당하는 Interface를 생성한다.
  3.	Interface내부에는 Query를 실행할 수 있는 메서드들을 넣는다.
  4.	Room DB에서 사용되는 Entity (model class) 를 data class 형식으로 만들어서 사용한다.
* 확장 함수 사용하기
  * 클래스에 새로 추가하고 싶은 기능이 있다면 확장 함수를 사용할 수 있다.
  * `fun String.isNumber(): Boolean { … } ` <- 기존 String클래스에는 isNumber()가 존재하지 않았지만 확장 함수를 통해 사용할 수 있게됨.
