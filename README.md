# Clean Code JavaScript

## فهرس المحتويات

1- [المقدمة](#المقدمة)

2- [المتغيرات](#المتغيرات)

3- [الدوال](#الدوال)

4- [الكائنات وهياكل البيانات](#الكائنات-وهياكل-البيانات)

5- [الفئات Classes](#الفئات-Classes)

6- [مصطلح SOLID](#مصطلح-SOLID)

7- [الاختبار](#الاختبار)

8- [التزامن](#التزامن)

9- [معالجة الأخطاء](#معالجة-الأخطاء)

10- [التنسيق](#التنسيق)

11- [التعليقات](#التعليقات)

12- [الترجمة](#الترجمة)

## المقدمة

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://raw.githubusercontent.com/muh-osman/clean-code-javascript-arabic/main/image.jpg)

مبادئ هندسة البرمجيات ، من كتاب روبرت سي مارتن [_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) ،
تم تكييفها مع JavaScript. هذا ليس دليل لأسلوب كتابة الكود. بل دليل لإنتاج
كود جافا سكريبت [قابل للقراءة ولإعادة الاستخدام والبناء](https://github.com/ryanmcdermott/3rs-of-software-architecture).

لا يجب اتباع جميع هذه المبادئ بدقة ، خصوصا أنها غير متفق عليها عالميا. هذه مجرد إرشادات لا أكثر من ذلك ، لكنها مقننة على مدى سنوات عديدة من الخبرة الجماعية لمؤلفي كتاب _Clean Code_.

يتجاوز عمر هندسة البرمجيات 50 عامًا بقليل ، وما زلنا نتعلم الكثير. عندما تكون هندسة البرمجيات قديمة قدم الهندسة المعمارية نفسها ، فربما سيكون لدينا قواعد أصعب يجب اتباعها. في الوقت الحالي ، استخدم هذه الإرشادات كمحك لتقييم جودة كود JavaScript الذي تنتجه أنت وفريقك.

شيء آخر يجيب أن تعرفه وهو أن هذه الارشادات لن تجعلك على الفور مطور أفضل ، والعمل بها لسنوات طويلة لا يعني أنك لن ترتكب أخطاء. تبدأ كل قطعة من الكود كمسودة أولى ، مثل تشكيل الطين في شكله النهائي. أخيرًا ، نزيل العيوب عندما نراجعها مع فريق العمل. لا تضغط على نفسك بسبب كثرة المسودات التي تحتاج إلى تحسين. تغلب على الكود بدلا من ذلك!

## **المتغيرات**

### #1 استخدم اسماء متغيرات ذات معنى وقابلة للنطق

**Bad⛔:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**Good✅:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

### #2 استخدم نفس الكلمات لنفس النوع من المتغيرات

**Bad⛔:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Good✅:**

```javascript
getUser();
```

### #3 استخدم أسماء قابلة للبحث

ستقرأ أكود أكثر مما ستكتب. لذا من المهم أن يكون الكود الذي تكتبه قابل للقراءة والبحث. فتجاهل تسمية المتغيرات الهامة لفهم برنامجك، ستؤذي قراءتك. اجعل أسماءك قابلة للبحث. يمكن أن تساعد أدوات مثل [buddy.js](https://github.com/danielstjules/buddy.js) و [ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) في تحديد الثوابت غير المسماة.

**Bad⛔:**

```javascript
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);
```

**Good✅:**

```javascript
// Declare them as capitalized named constants.
const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; //86400000;

setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

### #4 استخدم المتغيرات التوضيحية

**Bad⛔:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**Good✅:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

### #5 تجنب الخرائط الذهنية

الصريح أفضل من الضمني.

**Bad⛔:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Wait, what is `l` for again?
  dispatch(l);
});
```

**Good✅:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(location => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```

### #6 لا تقم بإضافة سياق غير ضروري

إذا كان اسم الـ class/object يدل على شيئ محدد، فلا تقم بتكراره في اسم المتغير.

**Bad⛔:**

```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car, color) {
  car.carColor = color;
}
```

**Good✅:**

```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car, color) {
  car.color = color;
}
```

### #7 استخدم parameter الافتراضي بدلاً من الاختصارات أو الشروط

غالبًا ما تكون الـ parameters  الافتراضية أنظف من الاختصارات. اعلم أنه في حالة استخدامها ، فإن وظيفتك ستقتصر على توفير القيم الافتراضية لـ arguments غير المعرفة (`undefined`). اما القيم الـ "falsy" مثل `""` و `""` و `false` و `null` و `0` و `NaN` فلن يتم استبدالها بقيمة افتراضية.

**Bad⛔:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**Good✅:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

## **الدوال**

### #8 عدد الـ arguments المثالي (2 أو أقل)

يعتبر الحد من عدد الـ parameters في الـ function أمرًا مهمًا للغاية لأنه يجعل اختبارالـ function أسهل. وبالعكس يؤدي وجود أكثر من ثلاثة إلى انفجار اندماجي حيث يتعين عليك اختبار العديد من الحالات المختلفة مع argument منفصلة.

الحالة المثالية هي أن يكون هناك واحد أو اثنين من الـ arguments، ويجب تجنب ثلاث arguments إن أمكن. أي شيء أكثر من ذلك يجب أن يتم توحيده ودمجه. عادة ، إذا كان لديك أكثر من وسيطين(arguments) ، فإن الـ function الخاصة بك تحاول أداء الكثير من الوظائف. في الحالات التي لا يكون فيها الأمر كذلك ، فإن الـ object ذي المستوى الأعلى يكفي في معظم الأحيان كوسيط(argument).

نظرًا لأن JavaScript تسمح لك بإنشاء object بشكل سريع، دون الحاجة الى الكثير من  الطبقات المعيارية، يمكنك استخدام الـ object إذا وجدت نفسك بحاجة إلى الكثير من الـ arguments.

لتوضيح الخصائص التي يمكن أن تتوقعها من الـ function، يمكنك استخدام صيغة ES2015 / ES6. وهذه بعض مزاياها:

1. عندما ينظر شخص ما إلى المعلومات العامة لـ function، يتضح على الفور ما هي الخصائص التي يتم استخدامها.
2. يمكن استخدامها لمحاكاة الـ parameters المسماة.
3. يؤدي النشر أيضًا إلى استنساخ القيم الأولية المحددة لـ argument object الذي يتم تمريره إلى function. يمكن هذا أن يساعد على منع الآثار الجانبية. ملاحظة: لا يتم نسخ objects وarrays التي تم نشرها من argument object.
4. يمكن أن تحذرك ادوات فحص الكود(Linters) من الخصائص غير المستخدمة ، والتي ستكون مستحيلة دون النشر.

**Bad⛔:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);

```

**Good✅:**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true
});
```

### #9 الـ Functions يجب أن تقوم بوظيفة واحدا

هذه هي القاعدة الأكثر أهمية في هندسة البرمجيات. عندما تقوم الـ Function بأكثر من وظيفة، يؤدي ذلك الى صعوبة تكوينها واختبارها وتفسيرها. بالتالي عندما تتمكن من عزل الـ Function لأداء وظيفة واحد فقط ، ستتمكن من إعادة بنائها بسهولة وستقرأ تعليماتها البرمجية بشكل أكثر وضوح. إذا أخذت هذه القاعدة بعين الاعتبار، فستتقدم على العديد من المطورين.

**Bad⛔:**

```javascript
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Good✅:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

### #10 اسم الـ Function يجب ان يدل على وظيفتها

**Bad⛔:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);
```

**Good✅:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

### #11 يجب أن تحوي الـ Functions مستوى واحد من الأكواد المجردة

عندما يكون لديك أكثر من مستوى واحد من المهام والوظائف التي يفترض بالـ function أن تأديها فهذا يعني أن الـ function عادة ما تنفذ الكثير من المهام. بالتالي فإن تقسيم هذه المهام والوظائف الى عدة function يؤدي إلى سهولة إعادة استخدام الـ functions واختبارها.

**Bad⛔:**

```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach(token => {
    // lex...
  });

  ast.forEach(node => {
    // parse...
  });
}
```

**Good✅:**

```javascript
function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);
  syntaxTree.forEach(node => {
    // parse...
  });
}

function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function parse(tokens) {
  const syntaxTree = [];
  tokens.forEach(token => {
    syntaxTree.push(/* ... */);
  });

  return syntaxTree;
}
```

### #12 أزل الأكواد المكررة

ابذل قصارى جهدك لتجنب تكرار الكود. الكود المكرر سيء لأن ذلك يعني أن هناك عدة أكواد في أماكن مختلفة يجب الوصول اليها وتعديلها اذا ما كنت بحاجة لتغيير بعض المنطق البرمجي لتطبيقك.

تخيل أنك تدير مطعمًا وتتابع مخزونك: كل الطماطم ، والبصل ، والثوم ، والتوابل ، وما إلى ذلك. إذا كانت لديك قوائم متعددة تحتفظ بها ، فيجب تحديثها جميعًا عند تقديم طبق فيه طماطم. أما إذا كان لديك قائمة واحدة فقط ، فهناك مكان واحد فقط لتعديله!

في كثير من الأحيان يكون لديك كود مكرر لأن لديك شيئين مختلفين أو أكثر قليلاً ، يشتركان في الكثير من القواسم المشتركة ، لكن الاختلافات بينهما تجبرك على وجود دالتين منفصلتين أو أكثر تقومان بالكثير من الأشياء نفسها. تعني إزالة الكود المكرر إنشاء تجريد يمكنه التعامل مع هذه المجموعة من الأشياء المختلفة بـ function/module/class واحدة فقط.

يعد الوصول الى التجريد بشكله الصحيح أمرًا بالغ الأهمية ، ولهذا السبب يجب عليك اتباع مبادئ SOLID الموضحة في قسم الـ _Classes_. يمكن أن تكون التجريدات السيئة أسوأ من الكود المكرر ، لذا كن حذرًا! بعد قولي هذا ، إذا كان بإمكانك عمل فكرة تجريدية جيدة ، فافعل ذلك! لا تكرر نفسك ، وإلا ستجد نفسك تقوم بتحديث الكود بأماكن متعددة في أي وقت تريد تغيير شيء واحد.

**Bad⛔:**

```javascript
function showDeveloperList(developers) {
  developers.forEach(developer => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**Good✅:**

```javascript
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    const data = {
      expectedSalary,
      experience
    };

    switch (employee.type) {
      case "manager":
        data.portfolio = employee.getMBAProjects();
        break;
      case "developer":
        data.githubLink = employee.getGithubLink();
        break;
    }

    render(data);
  });
}
```

### #13 قم بتعيين قيم افتراضية لـ Objects باستخدام Object.assign

**Bad⛔:**

```javascript
const menuConfig = {
  title: null,
  body: "Bar",
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || "Foo";
  config.body = config.body || "Bar";
  config.buttonText = config.buttonText || "Baz";
  config.cancellable =
    config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**Good✅:**

```javascript
const menuConfig = {
  title: "Order",
  // User did not include 'body' key
  buttonText: "Send",
  cancellable: true
};

function createMenu(config) {
  let finalConfig = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true
    },
    config
  );
  return finalConfig
  // config now equals: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

### #14 لا تستحدم الـ flags كا parameters لـ function

تخبر الـ Flags المستخدم أن هذه الـ function تقوم بأكثر من شيء. يجب أن تقوم الـ function بشيئ واحد. قسّم الـ function إذا كانت تتبع مسارات كود مختلفة بناءً على قيمة منطقية(true/false).

**Bad⛔:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Good✅:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

### #15 تجنب الآثار الجانبية (الجزء 1)

تنتج الدالة تأثيرًا جانبيًا إذا فعلت أي شيء آخر غير أخذ قيمة وإرجاع قيمة أو قيم أخرى. قد يكون أحد الآثار الجانبية هو الكتابة في ملف ، أو تعديل بعض المتغيرات العامة ، أو تحويل كل أموالك بطريق الخطأ إلى شخص غريب.

في بعض الأحيان، أنت بحاجة إلى أن يكون لديك آثار جانبية في تطبيقك. مثل المثال السابق ، قد تحتاج إلى الكتابة في ملف. ما تريد القيام به هو التركيز على مكان قيامك بذلك. ليس لديك العديد من functions و classes التي تكتب في ملف معين. لديك خدمة واحدة تفعل ذلك. واحد فقط لا غير.

النقطة الأساسية هي تجنب المطبات الشائعة مثل مشاركة الحالة بين الـ objects بدون أي بنية ، باستخدام أنواع البيانات القابلة للتعديل والتغير والتي يمكن تعديلها بأي شيء ، وليس التركيز على مكان حدوث الآثار الجانبية. إذا تمكنت من القيام بذلك ، فستكون أكثر سعادة من الغالبية العظمى من المبرمجين الآخرين.

**Bad⛔:**

```javascript
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Good✅:**

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

### #16 تجنب الآثار الجانبية (الجزء 2)

في JavaScript ، بعض القيم غير قابلة للتعديل (immutable) وبعضها قابل للتعديل (mutable). تعد الـ Objects والـ arrays نوعين من القيم القابلة للتعديل ، لذا من المهم التعامل معها بعناية عند تمريرها كا parameters إلى دالة. يمكن للـ function في JavaScript تغيير خصائص الـ  object أو تغيير محتويات المصفوفة مما قد يتسبب بسهولة في حدوث أخطاء في مكان آخر.

افترض أن هناك function تقبل مصفوفة كا parameter تمثل عربة تسوق. إذا قامت هذه الدالة بإجراء تغيير في محتويات مصفوفة عربة التسوق هذه - عن طريق إضافة عنصر للشراء ، على سبيل المثال - فإن أي وظيفة أخرى تستخدم نفس مصفوفة `سلة` التسوق هذه ستتأثر بهذه الإضافة. قد يكون هذا رائعًا ، ولكنه قد يكون سيئًا أيضًا. لنتخيل وضعًا سيئًا:

ينقر المستخدم على زر "شراء" الذي يستدعي دالة `الشراء` التي تولد طلب من الشبكة وترسل مصفوفة `سلة` التسوق إلى الخادم.لكن بسبب الاتصال السيئ بالشبكة ، يجب أن تستمر دالة `الشراء` في محاولة إعادة الطلب. الآن، ماذا لو نقر المستخدم في هذه الأثناء عن طريق الخطأ على زر "إضافة إلى عربة التسوق" على عنصر لا يريده بالفعل قبل بدء طلب الشبكة؟ إذا حدث ذلك وبدأ طلب الشبكة ، فستقوم دالة الشراء هذه بإرسال العنصر المضاف بطريق الخطأ لأنه تم تعديل مصفوفة `سلة` التسوق.

سيكون الحل الرائع لدالة `addItemToCart` أن تقوم دائمًا باستنساخ `سلة التسوق` وتحريرها وإرجاع النسخة المستنسخة. سيضمن ذلك أن الدالة التي لا تزال تستخدم عربة التسوق القديمة لن تتأثر بالتغييرات.

هناك تحذيران يجب ذكرهما لهذا النهج:

1. قد تكون هناك حالات تريد فيها بالفعل تعديل object الـ input ، ولكن عندما تتبنى هذا النهج في البرمجة، ستجد أن هذه الحالات نادرة جدًا. يمكن إعادة هيكلة معظم الاشياء حتى لا يكون لها أي آثار جانبية!
2. قد يكون استنساخ الـ objects الكبيرة مكلفًا للغاية من حيث الأداء. لكن لحسن الحظ ، هذه ليست مشكلة كبيرة من الناحية العملية لأن هناك [مكتبات رائعة](https://facebook.github.io/immutable-js/) تسمح لهذا النوع من نهج البرمجة أن يكون سريعًا وليس كثيفًا للذاكرة كما هو الحال بالنسبة لك عند استنساخ الـ objects و arrays يدويًا.

**Bad⛔:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Good✅:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

### #17 لا تقم بالكتابة على الـ functions العامة

يعد تلويث المجال العام ممارسة سيئة في جافا سكريبت لأنك قد تصطدم بمكتبة أخرى ولن يكون مستخدم الـ API الخاص بك أكثر حكمة حتى يحصل على استثناء في مرحلة الإنتاج النهائي للتطبيق. لنفكر في مثال: ماذا لو أردت توسيع الـ method الأصلية لـ Array في جافا سكريبت لتشمل method جديد اسمها `diff` وظيفتها اظهار الفرق بين مصفوفتين؟ يمكنك كتابة function جديدة في `Array.prototype` ، لكنها قد تتعارض مع مكتبة أخرى تحاول أن تفعل الشيء نفسه. ماذا لو كانت تلك المكتبة الأخرى تستخدم `diff` لإيجاد الفرق بين أول وآخر عنصر في المصفوفة؟ هذا هو السبب في أنه سيكون من الأفضل استخدام classes ES2015 / ES6 وتمديد مجال الـ `Array`.

**Bad⛔:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**Good✅:**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

### #18 البرمجة الوظيفية مفضلة على البرمجة الضرورية

من المعروف أن JavaScript ليست لغة وظيفية بالطريقة التي هي عليها لغات اخرى مثل Haskell ، لكن لها نكهة وظيفية. يمكن أن تكون اللغات الوظيفية أنظف وأسهل في الاختبار. لذا يفضل استخدام هذا النمط من البرمجة.

**Bad⛔:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**Good✅:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

const totalOutput = programmerOutput.reduce(
  (totalLines, output) => totalLines + output.linesOfCode,
  0
);
```

### #19 غلف الشروط

**Bad⛔:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**Good✅:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

### #20 تجنب الشروط المنفية

**Bad⛔:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Good✅:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

### #21 تجنب الشروط

عند سماع هذا لأول مرة، تبدو هذه مهمة مستحيلة. يقول معظم الناس: "كيف يفترض بي أن أفعل أي شيء بدون `if` الجواب هو أنه يمكنك استخدام تعدد الأشكال(Polymorphism) لتحقيق نفس المهمة في كثير من الحالات. السؤال الثاني: "حسنًا هذا رائع ولكن لماذا أرغب في القيام بذلك؟" الجواب هو مفهوم الكود النظيف السابق الذي تعلمناه يجب أن تقوم الـ function بشيء واحد فقط. عندما يكون لديك classes وfunctions  تحوي على عبارات `if`، فأنت تخبر المستخدم أن function  تقوم بأكثر من شيء واحد. تذكر ، فقط افعل شيئًا واحدًا.

**Bad⛔:**

```javascript
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case "777":
        return this.getMaxAltitude() - this.getPassengerCount();
      case "Air Force One":
        return this.getMaxAltitude();
      case "Cessna":
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**Good✅:**

```javascript
class Airplane {
  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

### #22 تجنب التحقق من النوع (الجزء 1)

بشكل افراضي JavaScript لا تكترث بالنوع، مما يعني أن الـ functions يمكن أن تأخذ أي نوع من الـ argument. أحيانًا تلدغك هذه الحرية ويصبح من المغري القيام بفحص النوع في الـ functions. هناك العديد من الطرق لتجنب الاضطرار إلى القيام بذلك. أول شيء يجب مراعاته هو الـ APIs الثابتة.

**Bad⛔:**

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**Good✅:**

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

### #23 تجنب التحقق من النوع (الجزء 2)

إذا كنت تعمل بقيم أولية أساسية مثل السلاسل والأعداد الصحيحة ، ولا يمكنك استخدام تعدد الأشكال(polymorphism) ولكنك ما زلت تشعر بالحاجة إلى التحقق من نوع البيانات، فعليك التفكير في استخدام TypeScript. إنه بديل ممتاز لجافا سكريبت الاساسية، لأنه يوفر لك كتابة ثابتة ضمن بناء جملة جافا سكريبت القياسي. تكمن مشكلة فحص JavaScript العادي يدويًا في أن القيام بذلك بشكل جيد يتطلب الكثير من الإسهاب الإضافي بحيث لا يعوض "أمان النوع" الزائف الذي تحصل عليه عن قابلية القراءة المفقودة. حافظ على نظافة JavaScript الخاصة بك ، واكتب اختبارات جيدة ، واحصل على تقييمات جيدة للكود. بخلاف ذلك ، افعل كل ذلك ولكن باستخدام TypeScript (والذي ، كما قلت ، هو بديل رائع!).

**Bad⛔:**

```javascript
function combine(val1, val2) {
  if (
    (typeof val1 === "number" && typeof val2 === "number") ||
    (typeof val1 === "string" && typeof val2 === "string")
  ) {
    return val1 + val2;
  }

  throw new Error("Must be of type String or Number");
}
```

**Good✅:**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

### #24 لا تفرط في تحسين الكود

تقوم المتصفحات الحديثة بالكثير من التحسين (optimization) في الخلفية. في كثير من الأحيان، إذا كنت تقوم بالتحسين ، فأنت تضيع وقتك فقط. [هناك موارد جيدة](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) لمعرفة الأماكن التي تفتقر إلى التحسين. استهدف هذه الاماكن في هذه الأثناء، حتى يتم إصلاحهم إذا أمكن ذلك.

**Bad⛔:**

```javascript
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Good✅:**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

### #25 أزل الأكواد الميتة

الأكواد الميتة سيئة مثل الأكواد المكررة. لا يوجد سبب للاحتفاظ بها في قاعدة البيانات الخاصة بك. إذا لم يتم استخدامه، فتخلص منه! سيظل آمنًا في محفوظات مستودعك إذا كنت لا تزال بحاجة إليه.

**Bad⛔:**

```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**Good✅:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

## **الكائنات وهياكل البيانات**

### #26 استخدم getters و setters

قد يكون استخدام getters و setters للوصول إلى بيانات الـ objects أفضل من مجرد البحث عن property في object ما. "لماذا؟" ربما تسال. حسنًا ، إليك قائمة غير منظمة بالأسباب:

- عندما تريد أكثر من مجرد الحصول على الـ property الخاصة بالـ object ، لا يتعين عليك البحث عن كل ملحق في قاعدة التعليمات البرمجية الخاصة بك وتغييره.
- يجعل إضافة التحقق من الصحة (validation) أمرًا بسيطًا عند اجراء `set`.
- يغلف التمثيل الداخلي.
- من السهل إضافة تسجيل (logging) ومعالجة الأخطاء عند اجراء getting و setting.
- يمكنك التحميل البطيء لـ object's properties الخاص بك ، دعنا نقول الحصول عليه من الخادم.

**Bad⛔:**

```javascript
function makeBankAccount() {
  // ...

  return {
    balance: 0
    // ...
  };
}

const account = makeBankAccount();
account.balance = 100;
```

**Good✅:**

```javascript
function makeBankAccount() {
  // this one is private
  let balance = 0;

  // a "getter", made public via the returned object below
  function getBalance() {
    return balance;
  }

  // a "setter", made public via the returned object below
  function setBalance(amount) {
    // ... validate before updating the balance
    balance = amount;
  }

  return {
    // ...
    getBalance,
    setBalance
  };
}

const account = makeBankAccount();
account.setBalance(100);
```

### #27 اجعل للـ objects أعضاء خاصون بها

يمكن تحقيق ذلك من خلال الإغلاق closures (لـ ES5 وما دون).

**Bad⛔:**

```javascript
const Employee = function(name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: undefined
```

**Good✅:**

```javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    }
  };
}

const employee = makeEmployee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
```

## **الفئات Classes**

### #28 استخدم ES2015 / ES6 classes بدلا من ES5 functions

في ES5 classes الكلاسيكية من الصعب جدًا الحصول على class موروث سهل القرءة و قابل للبناء عليه وبـ method معرفة. إذا كنت بحاجة إلى التوريث (وكن على دراية بأنك قد لا تفعل ذلك) ، فعندئذ يفضل استخدام classes ES2015 / ES6. ومع ذلك ، يفضل استخدام الـ functions الصغيرة بدلا من classes حتى تجد نفسك بحاجة إلى objects أكبر وأكثر تعقيدًا.

**Bad⛔:**

```javascript
const Animal = function(age) {
  if (!(this instanceof Animal)) {
    throw new Error("Instantiate Animal with `new`");
  }

  this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function(age, furColor) {
  if (!(this instanceof Mammal)) {
    throw new Error("Instantiate Mammal with `new`");
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function(age, furColor, languageSpoken) {
  if (!(this instanceof Human)) {
    throw new Error("Instantiate Human with `new`");
  }

  Mammal.call(this, age, furColor);
  this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

**Good✅:**

```javascript
class Animal {
  constructor(age) {
    this.age = age;
  }

  move() {
    /* ... */
  }
}

class Mammal extends Animal {
  constructor(age, furColor) {
    super(age);
    this.furColor = furColor;
  }

  liveBirth() {
    /* ... */
  }
}

class Human extends Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor);
    this.languageSpoken = languageSpoken;
  }

  speak() {
    /* ... */
  }
}
```

### #29 استخدم method متسلسلة

هذا النمط مفيد جدًا في JavaScript ويمكنك رؤيته في العديد من المكتبات مثل jQuery و Lodash. يسمح للكود أن يكون معبر، وأقل إسهابًا. لهذا السبب ، أقول ، استخدم method متسلسلة وألق نظرة على مدى نظافة الكود الخاص بك. في class functions ، ما عليك سوى إرجاع `this` في نهاية كل function، ويمكنك ربط المزيد من class methods بها.

**Bad⛔:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
  }

  setModel(model) {
    this.model = model;
  }

  setColor(color) {
    this.color = color;
  }

  save() {
    console.log(this.make, this.model, this.color);
  }
}

const car = new Car("Ford", "F-150", "red");
car.setColor("pink");
car.save();
```

**Good✅:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // NOTE: Returning this for chaining
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOTE: Returning this for chaining
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOTE: Returning this for chaining
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOTE: Returning this for chaining
    return this;
  }
}

const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```

### #30 استخدم التأليف بدلا من التوريث

كما هو مذكور في كتاب [_Design Pattern_](https://en.wikipedia.org/wiki/Design_Patterns) لـ Gang of Four ، يجب أن تفضل التأليف على التوريث حيث يمكنك ذلك. هناك الكثير من الأسباب الوجيهة لاستخدام التوريث والعديد من الأسباب الوجيهة لاستخدام التأليف. النقطة الأساسية في هذا المبدأ هي أنه إذا ذهب عقلك غريزيًا إلى التوريث، فحاول التفكير فيما إذا كان التأليف يمكن أن يصوغ مشكلتك بشكل أفضل. في بعض الحالات يمكن ذلك.

قد تتساءل إذا، "متى يمكنني استخدام التوريث؟" يعتمد الأمر على مشكلتك، ولكن هذه قائمة جيدة عندما يكون التوريث أكثر منطقية من التأليف:

1. يمثل التوريث الخاص بك "is-a" علاقة "has-a" علاقة (الإنسان-> الحيوان VS  المستخدم-> تفاصيل المستخدم).
2. يمكنك إعادة استخدام التعليمات البرمجية من classes الأساسية (يمكن للبشر التحرك مثل جميع الحيوانات).
3. تريد إجراء تغييرات عامة على الـ classes المستمدة عن طريق تغيير class الأساسية. (تغيير عدد السعرات الحرارية التي يتم حرقها لجميع الحيوانات عندما تتحرك).

**Bad⛔:**

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Bad because Employees "have" tax data. EmployeeTaxData is not a type of Employee
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**Good✅:**

```javascript
class EmployeeTaxData {
  constructor(ssn, salary) {
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}

class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  setTaxData(ssn, salary) {
    this.taxData = new EmployeeTaxData(ssn, salary);
  }
  // ...
}
```

## **مصطلح SOLID**

### #31 مبدأ المسؤولية الفردية (SRP)

كما هو مذكور في كتاب Clean Code، "يجب ألا يكون هناك أكثر من سبب لتغيير الـ class". من المغري أن تحزم الـ class بالكثير من الوظائف، مثل حقيبة السفر الواحدة التي تأخذها عند سفرك. لكن المشكلة تكمن في أن الـ class لن يكون متماسكًا من الناحية المنطقية وسيكون هناك العديد من الأسباب للتغيير. من المهم تقليل عدد المرات التي تحتاج فيها لتغيير الـ class. إنه أمر مهم لأنه إذا كان هناك الكثير من الوظائف في class واحد وقمت بتعديل جزء منه، فقد يكون من الصعب فهم كيف سيؤثر ذلك على الوحدات(modules) التابعة الأخرى في قاعدة التعليمات البرمجية الخاصة بك.

**Bad⛔:**

```javascript
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**Good✅:**

```javascript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}

class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

### #32 مبدأ الفتح / الاغلاق (OCP)

كما ذكر Bertrand Meyer، "يجب أن تكون الاكواد البرمجية (classes, modules, functions، وما إلى ذلك) مفتوحة للتمديد ، ولكنها مغلقة للتعديل." ماذا يعني ذلك؟ ينص هذا المبدأ بشكل أساسي على أنه يجب عليك السماح للمستخدمين بإضافة وظائف جديدة دون تغيير التعليمات البرمجية الحالية.

**Bad⛔:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    if (this.adapter.name === "ajaxAdapter") {
      return makeAjaxCall(url).then(response => {
        // transform response and return
      });
    } else if (this.adapter.name === "nodeAdapter") {
      return makeHttpCall(url).then(response => {
        // transform response and return
      });
    }
  }
}

function makeAjaxCall(url) {
  // request and return promise
}

function makeHttpCall(url) {
  // request and return promise
}
```

**Good✅:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then(response => {
      // transform response and return
    });
  }
}
```

### #33 مبدأ Liskov للاستبدال (LSP)

هذا مصطلح مخيف لمفهوم بسيط للغاية. يتم تعريفه رسميًا على أنه "إذا كان S نوعًا فرعيًا من T ، فيمكن استبدال objects من النوع T بـ objects من النوع S (على سبيل المثال ، قد تحل objects من النوع S محل objects من النوع T) دون تغيير أي من الخصائص المرغوبة لهذا البرنامج (تصحيح، اجراء مهمة، إلخ). " هذا تعريف أكثر ترويعًا.

أفضل تفسير لذلك هو إذا كان لديك class أب وclass ابن، فيمكن استخدام الـ class الاب والـ class الابن بالتبادل دون الحصول على نتائج غير صحيحة. قد يكون هذا محيرًا ، لذلك دعونا نلقي نظرة على مثال "المستطيل- المربع" الكلاسيكي. من الناحية الرياضية ، يعتبر المربع مستطيل، ولكن إذا قمت بنمذجه باستخدام علاقة "is-a" عبر الوراثة، فإنك ستقع في مشكلة بسرعة.

**Bad⛔:**

```javascript
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach(rectangle => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // Bad⛔: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Good✅:**

```javascript
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach(shape => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

### #34 مبدأ فصل الواجهات (ISP)

لا تحتوي JavaScript على واجهات ، لذا فإن هذا المبدأ لا ينطبق بشكل صارم مثل اللغات الاخرى. ومع ذلك ، فهي مهمة وذات صلة حتى مع افتقار JavaScript إلى نظام للكتابة.

مبدأ الفصل بين الواجهات ISP ينص على أنه "لا ينبغي إجبار العملاء على الاعتماد على الواجهات التي لا يستخدمونها." الواجهات هي عقود ضمنية في JavaScript بسبب نمط الكتابة "duck typing".

من الأمثلة الجيدة التي يجب النظر إليها والتي توضح هذا المبدأ في JavaScript هي الـ classes التي تتطلب objects تحوي إعدادات كثيرة. يعد عدم مطالبة العملاء بإعداد كميات هائلة من الخيارات أمرًا مفيدًا ، لأنهم في معظم الأوقات لن يحتاجوا إلى جميع هذه الإعدادات. يساعد جعلها اختيارية في منع الوصول الى "واجهة سمينة".

**Bad⛔:**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.settings.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  animationModule() {} // Most of the time, we won't need to animate when traversing.
  // ...
});
```

**Good✅:**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.options = settings.options;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  options: {
    animationModule() {}
  }
});
```

### #35 مبدأ الاعتماد العكسي (DIP)

ينص هذا المبدأ على أمرين أساسيين:

1. يجب ألا تعتمد الـ modules عالية المستوى على modules المستوى المنخفض. كلاهما يجب أن يعتمد على الأكواد المجردة.
2. يجب ألا تعتمد الأكواد المجردة على التفاصيل. بل يفترض بالتفاصيل أن تعتمد على الأكواد المجردة.

قد يكون من الصعب فهم هذا في البداية ، ولكن إذا كنت قد استخدمت AngularJS ، فقد رأيت تطبيقًا لهذا المبدأ في شكل Dependency Injection (DI). على الرغم من أنها ليست مفاهيم متطابقة ، إلا أن DIP تمنع الـ modules عالية المستوى من معرفة تفاصيل الـ modules ذات المستوى المنخفض وإعدادها.  فائدة كبيرة أخرى لهذا المبدأ هو أنه يقلل من الاقتران بين الـ modules. يعتبر الاقتران نمط تطوير سيئً للغاية لأنه يجعل من الصعب إعادة بناء الكود الخاص بك.

كما ذكرنا سابقًا ، لا تحتوي JavaScript على واجهات ، لذا فإن الاكواد المجردة التي تعتمد عليها هي عقود ضمنية. وهذا يعني أن الـ methods و properties يعرضها object/class  لـ object/class  أخرى. في المثال أدناه، العقد الضمني يعني أن أي طلب من قبل module لـ `InventoryTracker` سيكون لها `requestItems` method.

**Bad⛔:**

```javascript
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;

    // Bad⛔: We have created a dependency on a specific request implementation.
    // We should just have requestItems depend on a request method: `request`
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(["apples", "bananas"]);
inventoryTracker.requestItems();
```

**Good✅:**

```javascript
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ["WS"];
  }

  requestItem(item) {
    // ...
  }
}

// By constructing our dependencies externally and injecting them, we can easily
// substitute our request module for a fancy new one that uses WebSockets.
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

## **الاختبار**

إذا لم تقم بإختبار للكود أو لم تقم بذلك بشكل كافي، ففي كل مرة تقوم فيها بتشغيله لن تكون متأكدًا من أن تطبيقك سيسير في اتجاه خاطئ أم لا. إن اتخاذ قرار بشأن عدد الاختبارات المناسب متروك لفريقك ، لكن الحصول على تغطية بنسبة 100٪ (جميع الأكواد والفروع) هذا يعني تحقيق ثقة عالية جدًا وراحة بال للمطور. الأمر الذي يتطلب بالإضافة إلى وجود إطار عمل رائع للاختبار(testing framework)، فإنك تحتاج أيضًا إلى استخدام [coverage tool](https://gotwarlost.github.io/istanbul/) كذلك.

ليس هناك أي عذر لعدم اجراء الاختبارات. هناك [الكثير من أطر عمل اختبار JS جيدة](https://jstherightway.org/#testing-tools)، لذا ابحث عن إطار يفضله فريقك. عندما تجد واحدًا يناسب فريقك، فهدف دائمًا إلى كتابة اختبارات لكل feature/module جديدة تقدمها. إذا كانت طريقتك المفضلة هي "التطوير المدفوع بالاختبار" (TDD) ، فهذا أمر رائع ، ولكن النقطة الأساسية هي التأكد من وصولك إلى أهداف التغطية قبل إطلاق أي ميزة، أو إعادة هيكلة ميزة موجودة.

### #36 مفهوم واحد لكل اختبار

**Bad⛔:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles date boundaries", () => {
    let date;

    date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);

    date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);

    date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**Good✅:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles 30-day months", () => {
    const date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);
  });

  it("handles leap year", () => {
    const date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);
  });

  it("handles non-leap year", () => {
    const date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

## **التزامن**

### #37 استخدم Promises، وليس callbacks

لا تعطي الـ Callbacks كود نظيف، وتسبب تداخلًا مفرطًا. بينما يعد استخدام الـ Promises المدمجة بـ ES2015 / ES6 ، نمطا عالميًا. استخدمه!

**Bad⛔:**

```javascript
import { get } from "request";
import { writeFile } from "fs";

get(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  (requestErr, response, body) => {
    if (requestErr) {
      console.error(requestErr);
    } else {
      writeFile("article.html", body, writeErr => {
        if (writeErr) {
          console.error(writeErr);
        } else {
          console.log("File written");
        }
      });
    }
  }
);
```

**Good✅:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

### #38 استخدام Async / Await أنظف حتى من الوعود(Promises)

تعد الوعود بديلاً نظيفًا جدًا لـ callbacks، لكن ES2017 / ES8 تجلب async و await والتي تقدم حلاً أكثر نظافة. كل ما تحتاجه هو function مسبوقة بكلمة `async`، وبعد ذلك يمكنك كتابة الكود البرمجي دون الحاجة الى سلسلة من الـ `then`. استخدم هذا من الأن إذا كان بإمكانك الاستفادة من ميزات ES2017 / ES8!

**Bad⛔:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**Good✅:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

async function getCleanCodeArticle() {
  try {
    const body = await get(
      "https://en.wikipedia.org/wiki/Robert_Cecil_Martin"
    );
    await writeFile("article.html", body);
    console.log("File written");
  } catch (err) {
    console.error(err);
  }
}

getCleanCodeArticle()
```

## **معالجة الأخطاء**

الأخطاء الظاهرة شيء جيد! إنها تعني أنه اثناء تشغيل الكود قد تم تحديد حدوث خطأ ما ويخبرك ذلك عن طريق إيقاف تنفيذ الـ function، وايقاف العملية (في الـ Node) ، وإخطارك في الـ console لتتبع الخطأ.

### #39 لا تتجاهل الأخطاء المكتشفة

عدم التعامل مع خطأ تم اكتشافه لا يمنحك القدرة على إصلاح أو الرد على الخطأ المذكور. كما لا يعد تسجيل الخطأ في الـ (`console.log`) أفضل بكثير لأنه في كثير من الأحيان يمكن أن تضيع في بحر من الأشياء المطبوعة في الـ console. لكن إذا قمت بإحاطة أي جزء من الكود بـ `try/catch`، فهذا يعني أنك تعتقد أن خطأ ما قد يحدث هناك ، وبالتالي يجب أن يكون لديك خطة ، أو إنشاء مسار مختلف، عندما يحدث.

**Bad⛔:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**Good✅:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // One option (more noisy than console.log):
  console.error(error);
  // Another option:
  notifyUserOfError(error);
  // Another option:
  reportErrorToService(error);
  // OR do all three!
}
```

### #40 لا تتجاهل الوعود المرفوضة

لنفس السبب لا يجب أن تتجاهل الأخطاء التي يتم اكتشافها من `try/catch`.

**Bad⛔:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**Good✅:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    // One option (more noisy than console.log):
    console.error(error);
    // Another option:
    notifyUserOfError(error);
    // Another option:
    reportErrorToService(error);
    // OR do all three!
  });
```

## **التنسيق**

تنسيق الكود هو امر شخصي. مثل العديد من القواعد الواردة هنا، لا توجد قاعدة صارمة وسريعة يجب عليك اتباعها. النقطة الأساسية هي عدم الجدال حول التنسيق. هناك [الكثير من الأدوات](https://standardjs.com/rules.html) لأتمتة هذا. استخدم واحدة! الجدل حول التنسيق يعد مضيعة للوقت والمال.

بالنسبة للأشياء التي لا تقع ضمن نطاق التنسيق التلقائي (المسافة البادئة ، tabs vs. spaces، علامات الاقتباس المزدوجة مقابل علامات الاقتباس المفردة ، إلخ) ، ابحث هنا عن بعض الإرشادات.

### #41 اكتب بأحرف كبيرة وبطريقة متسقة

تعد JavaScript لغة غير مطبوع (untyped)، لذا فإن الكتابة بالأحرف الكبيرة تخبرك كثيرًا عن المتغيرات والـ functions وما إلى ذلك. هذه القواعد شخصية، لذا يمكن لفريقك اختيار ما يريده. النقطة المهمة هي، بغض النظر عما تختاروه جميعًا ، فقط كن متسقًا.

**Bad⛔:**

```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const Artists = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**Good✅:**

```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const ARTISTS = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```

### #42 يجب على الـ Function التي تستدعي أخرى أن تكون قريبة منها

إذا استدعت دالة دالة أخرى ، ابقي هذه الـ functions قريبة من بعضها البعض عموديًا في الملف المصدر. من الناحية المثالية ، احتفظ بالمستدعي فوق المستدعى مباشرةً. نميل إلى قراءة التعليمات البرمجية من أعلى إلى أسفل ، مثل الجريدة. لهذا السبب ، اجعل الكود الخاص بك يقرأ بهذه الطريقة.

**Bad⛔:**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**Good✅:**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

## **التعليقات**

### #43 قم فقط بالتعليق على الأكواد التي تحوي منطق معقد

التعليقات هي تبرير وليست شرطا اساسيا. الكود الجيد _في الغالب_   يوثق نفسه بنفسه.

**Bad⛔:**

```javascript
function hashIt(data) {
  // The hash
  let hash = 0;

  // Length of string
  const length = data.length;

  // Loop through every character in data
  for (let i = 0; i < length; i++) {
    // Get character code.
    const char = data.charCodeAt(i);
    // Make the hash
    hash = (hash << 5) - hash + char;
    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**Good✅:**

```javascript
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

### #44 لا تترك التعليمات البرمجية المعلقة في ملفك

نظام التحكم في الإصدار موجود لهذا السبب. اترك الكود القديم في سجلك.

**Bad⛔:**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Good✅:**

```javascript
doStuff();
```

### #45 لا تكثر من التعليقات

تذكر، استخدم تطبيقات التحكم في الإصدار مثل git! ليست هناك حاجة للأكواد الميتة ، والتعليمات البرمجية المعلقة ، وخاصة التعليقات الكثيرة. استخدم `git log` للحصول على قائمة بجميع التغييرات!

**Bad⛔:**

```javascript
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Removed type-checking (LI)
 * 2015-03-14: Added combine with type-checking (JR)
 */
function combine(a, b) {
  return a + b;
}
```

**Good✅:**

```javascript
function combine(a, b) {
  return a + b;
}
```

### #46 تجنب العلامات الموضعية

عادة ما يضيفون الكثير من التعليقات الفوضوية. فقط دع الـ functions وأسماء المتغيرات جنبًا إلى جنب مع المسافة البادئة المناسبة والتنسيق سيعطي البنية المرئية للكود الخاص بك.

**Bad⛔:**

```javascript
////////////////////////////////////////////////////////////////////////////////
// Scope Model Instantiation
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: "foo",
  nav: "bar"
};

////////////////////////////////////////////////////////////////////////////////
// Action setup
////////////////////////////////////////////////////////////////////////////////
const actions = function() {
  // ...
};
```

**Good✅:**

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar"
};

const actions = function() {
  // ...
};
```

## الترجمة

هذا الملف متوفر بلغات أخرى:

- ![am](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Armenia.png) **Armenian**: [hanumanum/clean-code-javascript/](https://github.com/hanumanum/clean-code-javascript)
- ![bd](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bangladesh.png) **Bangla(বাংলা)**: [InsomniacSabbir/clean-code-javascript/](https://github.com/InsomniacSabbir/clean-code-javascript/)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Simplified Chinese**:
  - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Traditional Chinese**: [AllJointTW/clean-code-javascript](https://github.com/AllJointTW/clean-code-javascript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [eugene-augier/clean-code-javascript-fr](https://github.com/eugene-augier/clean-code-javascript-fr)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
- ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Indonesia**: [andirkh/clean-code-javascript/](https://github.com/andirkh/clean-code-javascript/)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [frappacchio/clean-code-javascript/](https://github.com/frappacchio/clean-code-javascript/)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/clean-code-javascript/](https://github.com/mitsuruog/clean-code-javascript/)
- ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
- ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [greg-dev/clean-code-javascript-pl](https://github.com/greg-dev/clean-code-javascript-pl)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**:
  - [BoryaMogila/clean-code-javascript-ru/](https://github.com/BoryaMogila/clean-code-javascript-ru/)
  - [maksugr/clean-code-javascript](https://github.com/maksugr/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [tureey/clean-code-javascript](https://github.com/tureey/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **Spanish**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
- ![rs](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Serbia.png) **Serbian**: [doskovicmilos/clean-code-javascript/](https://github.com/doskovicmilos/clean-code-javascript)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [bsonmez/clean-code-javascript](https://github.com/bsonmez/clean-code-javascript/tree/turkish-translation)
- ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **Ukrainian**: [mindfr1k/clean-code-javascript-ua](https://github.com/mindfr1k/clean-code-javascript-ua)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [hienvd/clean-code-javascript/](https://github.com/hienvd/clean-code-javascript/)
- ![ar](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Palestine.png) **Arabic**: [muh-osman/clean-code-javascript-arabic](https://github.com/muh-osman/clean-code-javascript-arabic)
