# EmptyGrid

პრობლემა:
```
ამოცანის პირობა:
  დახატეთ 10x10-ზე ხაზების ბადე გრაფიკულ პროგრამაში, ისე, რომ მთელი ფანჯარა დაიკავოს და ხაზებს შორის თანაბარი მანძილი იყო. 
```

## როგორ გადავჭრათ პრობლემა?
პირველ რიგში განვიხილოთ პრობლემის გადაჭრის ეტაპები:
1. ხაზებს შორის დისტანციის დათვლა.
2. ბადის დახატვა.


### ხაზებს შორის დისტანციის დათვლა
პირობის თანახმად, ბადეზე უნდა იყოს 10 სტრიქონი და 10 სვეტი. 
ხაზები უნდა გრძელდებოდეს მთლიან პროგრამის ფანჯარაზე, ამიტომ ჯერ დაგვჭირდება გამოვთვალოთ რა ზომისაა იგი.
ამისათვის მეთოდები `getWidth()` და `getHeight()` გამოვიყენოთ. თითოეულის 10-ზე გაყოფით მივიღებთ ზუსტ რიცხვს, თუ რა დაშორებით უნდა დავხატოთ
ხაზები, რომ მთლიანი ფანჯარა მოვიცვათ და 10x10 ბადე მივიღოთ.

```		
/* Calculate a gap between each row and column */ 
int rowGap = getHeight() / CELLS; 
int columnGap = getWidth() / CELLS;
```

### ბადის დახატვა
ბადის დახატვას 2 `for` ციკით შევძლებთ - პირველი სტრიქონებს, ხოლო მეორე სვეტებს დახატავს.

#### სტრიქონების დახატვა
GLine-ს უნდა გადავცეთ  საწყისი და საბოლოო წერტილების კოორდინატები.
- y კოორდინატი სტრიქონს საწყის და საბოლოო წერტილში  იგივე უნდა ჰქონდეს, რადგანაც იგი ჰორიზონალური მონაკვეთია.
- y კოორდინატის გამოთვლა მარტივად - `row * rowGap`-ით შეგვიძლია, სადაც row - for loop-ში შექმნილი ცვლადია, რომელიც კონკრეტული სტრიქონის ნომერს ასახავს, rowGap-კი სტრიქონებს შორის დაშორებაა.
- x0 კოორდინატი ყველა სტრიქონში 0 იქნება, x1 კი ფანჯრის სიგანე `getWidth()`, რადგანაც ხაზი  მთლიანი ფანჯრის გაყოლებაზე უნდა იყოს.

GLine-ის ინიციალიზებას, რომელიც სტრიქონებს დახაზავს, ასეთი სახე ექნება (CELLS ინახავს  უჯრების რაოდენობას  - 10):

```java
for(int row = 0; row < CELLS; row++) { 			
 	/* Coordinates */ 
 	double x0 = 0;
 	double x1 = getWidth();
 	double y = row * rowGap;
 		
 	GLine line = new GLine(x0, y, x1, y); // Initialise GLine object.
 	add(line); // Adds a line on the canvas.
}
 		
```

#### სვეტების დახატვა
GLine-ს უნდა გადავცეთ  საწყისი და საბოლოო წერტილების კოორდინატები.
- x კოორდინატი სტრიქონს საწყის და საბოლოო წერტილში  იგივე უნდა ჰქონდეს, რადგანაც იგი ვერტიკალური  მონაკვეთია.
- x კოორდინატის გამოთვლა მარტივად - `col * columnGap`-ით შეგვიძლია, სადაც col - for loop-ში შექმნილი ცვლადია, რომელიც კონკრეტული სვეტის  ნომერს ასახავს, columnGap-კი სვეტებს შორის დაშორებაა.
- y0 კოორდინატი ყველა სტრიქონში 0 იქნება, y1 კი ფანჯრის სიმაღლე `getWidth()`, რადგანაც ხაზი  მთლიანი ფანჯრის გაყოლებაზე უნდა იყოს.

GLine-ის ინიციალიზებას, რომელიც სვეტებს  დახაზავს, ასეთი სახე ექნება:

```java
for(int col = 0; col < CELLS; col++) {
 			
 	/* Coordinates */
 	double y0 = 0;
 	double y1 = getHeight();
 	double x = col * columnGap;

 	GLine line = new GLine(x, y0, x, y1); // Initialise GLine object.
 	add(line); // Add line on the canvas.
}
 		
```


### ამოხსნის ალტერნატიური გზა
პირობაში მოცემულია, რომ ბადე უნდა დაიხაზოს ხაზებით, მაგრამ შეგვიძლია ვცადოთ ბადის მართკუთხედებით დახაზვაც.

ამ შემთხვევაში ჩადგმულ ციკლს გამოვიყენებთ - ერთ `for`ში მეორეს ჩავდგამთ, რადგან მართკუთხედის დასახატად ერთდროულად სვეტებსა და სტრიქონებზე  დაგვჭირდება  ინფორმაცია.
`nested for loop` ასე გამოიყურება:

```java
for(int row = 0; row < CELLS; row++) {
 	for(int col = 0; col < CELLS; col++) {
 	}
}
```

მართკუთხდების დასახატად `GRect` კლასი გამოვიყენოთ რომელსაც 4 პარამეტრს გადავცემთ:
- `width` - ჩვენი უკვე გამოთვლილი დაშორება სვეტებს შორის.
- `height` - ასევე უკვე გამოთვლილი დაშორება სტრიქონებს შორის.
- `x` - `col * width`
- `y` - `row * height`

col და row არის შესაბამისად for ციკლში სვეტისა და სტრიქონის ნომერი, რომლის კოორდინატებსაც ვითვლით.


```java
for(int row = 0; row < CELLS; row++) { // For each row
    for(int col = 0; col < CELLS; col++) { // For each column	
 		/* Coordinates and size*/ 
 		double width = columnGap;
 		double height = rowGap;
		double x = col * width;
		double y = row * height;

 		GRect rect = new GRect(x, y, width, height); // Initialise GRect object.
 		add(rect); // Add rectangle on the canvas.
	}
}
```