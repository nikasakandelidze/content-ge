# Pencil

პრობლემა:
```
გააკეთეთ Paint-ის ფანქრის ანალოგიური ხელსაწყო. კერძოდ, მაუსის ყოველ მოძრაობაზე
ეკრანზე დაამატეთ გაფერადებული წრეწირები იმავე წერტილში სადაც მაუსი მდებარეობს.
```



## პრობლემის გადაჭრის გზა:
ამოცანის ამოსახსნელად გამოვიყენებთ Event-ებს:
 *mouseMoved() მეთოდში დავაიმპლემენტირებთ ჩვენი წრეწირის დახატვის ლოგიკას

---

### I. ინიციალიზაციის ეტაპი:
 * ვიდრე გამოვიყენებდეთ mouse event-ების მეთოდებს, საჭიროა, რომ პროგრამამ ყურადღება მიაქციოს, ანუ "მოუსმინოს" ჩვენს mouse event-ებს, ამისათვის init()-მეთოდში, რომელიც ერთხელ გამოიძახება run() მეთოდამდე, რათა გარკვეულ ველებს ინიციალიზება გავუკეთოთ, ჩავწეროთ addMouseListeners(), რომ ამის შემდგომ პროგრამას mouse-ის event-ებზე რეაგირება მოახდინოს.

### II. რომელ mouse event-ებს გამოვიყენებთ:
 * რადგან მაუსის ყოველ მოძრაობაზე უნდა დავხატოთ წრეწირი, ამიტომ დავწეროთ წრეწირის დახატვის ლოგიკა mouseMoved() მეთოდში, mouseMoved()-ის გარდა გვაქვს mousePressed() (სრულდება ერთხელ, მაუსის დაჭერის მომენტში), mouseClicked()(სრულდება, მაუსის დაჭერის შემდგომ აშვების შემდეგ), mouseDragged()(სრულდება, როდესაც მაუსი დაჭერილი გვაქვს და თან ვამოძრავებთ, ყოველი მოძრაობისას ხელახლა იძახება ეს მეთოდი), mouseReleased()(სრულდება, როდესაც მაუსს ვუშვებთ).

### III. MouseEvent კლასი :
 * ჩამოთვლილი mouse event-ების მეთოდებს ყველას გადაეცემა პარამეტრად MouseEvent კლასის ტიპის ობიექტი e (სახელი ნებისმიერია). ამ ობიექტის მეშვეობით შეგვიძლია მივიღოთ მრავალი ტიპის ინფორმაცია მომხდარ event-ზე, მაგალითად, თუ რომელ კოორდინატებზე მოხდა ის(e.getX() და e.getY()), რა ობიექტზე მოხდა ეს event (e.getSource(), რომელიც ჩვენ შემთხვევაში კანვასს დაგვიბრუნებს, რადგანაც მაუსს კანვასზე ვამოძრავებთ), დრო თუ როდის მოხდა ეს event(e.getWhen(), რომელიც გვიბრუნებს განვლილ დროს მილიწამებში 1970წლის 1 იანვრის 00:00UTC) და ა.შ.

### IV. მაუსის გამოძრავებისას წრეწირების დახატვა :

 * როგორც ვიცით, mouseMoved() მეთოდი მაუსის ყოველი გამოძრავებისას იძახება, ამიტომ ჩვენი მთლიანი ამოცანა დავიდა იმაზე, რომ ამ მეთოდში უბრალოდ წრეწირი დავხატოთ და ამის შემდგომ მაუსის ყოველ გამოძრავებაზე ხელახლა დაიხატება წრეწირი. ვქმნით GOval ტიპის ობიექტს, რომელსაც კოორდინატებად გადავცემთ e.getX() და e.getY() მეთოდებს, ხოლო დიამეტრისათვის გლობალურად აღვწეროთ ცვლადი DIAMETER და ის გადავცეთ. იმისათვის, რომ უკეთ გამოჩნდეს ჩვენი დახატული წრეწირები, გავხადოთ იგი ლურჯი ფერის, ამისათვის გამოვიყენოთ setFillColor() მეთოდი და პარამეტრად გადავცეთ Color.BLUE, და არ დაგვავიწყდეს setFilled(true)-ს დაწერა, რათა გაფერადებული იყოს ჩვენი წრეწირი, შედეგად მივიღებთ კოდს :

```java
public void mouseMoved(MouseEvent e) {
	GOval oval = new GOval(e.getX(),e.getY(),DIAMETER,DIAMETER);
	oval.setFillColor(Color.BLUE);
	oval.setFilled(true); 
	add(oval);
}
```