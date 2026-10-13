# F.CSA313 — Лаборатори №6: Тест тохиолдол боловсруулах — Category-Partition ба PICT

> **B232270019 О. Оюунжаргал**
## Хэсэг A. Category-Partition

### A1. Сонголт ба төлөөлөх утгууд

| Параметр | Сонголт | Төлөөлөх утга |
|---|---|---|
| From | Хүчинтэй данс | Valid |
| From | Царцаагдсан данс | Frozen |
| From | Байхгүй данс | Missing |
| To | Хүчинтэй өөр данс | Valid |
| To | Байхгүй данс | Missing |
| To | From-той ижил данс | SameAsFrom |
| Amount | Тэг буюу тэгээс бага | Zero |
| Amount | Хэвийн дүн | Normal |
| Amount | Үлдэгдлээс их | OverBalance |
| Amount | Өдрийн лимитээс их | OverLimit |
| Currency | Дансны валюттай ижил | Same |
| Currency | Өөр валют | Different |

**Далд орчны сонголтууд:** дансны үлдэгдэл, өдрийн лимитийн үлдсэн хэмжээ, валют хөрвүүлэх ханш. Amount-ийн үлдэгдэл болон лимиттэй харьцах төлөвийг хөрвүүлсний дараах дүнгээр тодорхойлно.

### A2. Хязгаарлалт ба үндэслэл

| Сонголт | Тэмдэглэгээ | Үндэслэл |
|---|---|---|
| From = Valid | [SINGLE] | Хэвийн шилжүүлэгт хүчинтэй эх данс шаардлагатай. |
| From = Frozen | [ERROR] | Царцаагдсан данснаас шилжүүлэг хийх ёсгүй. |
| From = Missing | [ERROR] | Байхгүй эх данснаас шилжүүлэг хийх боломжгүй. |
| To = Valid | [SINGLE] | Амжилттай шилжүүлэгт хүчинтэй хүлээн авагч шаардлагатай. |
| To = Missing | [ERROR] | Хүлээн авагчийн данс олдохгүй. |
| To = SameAsFrom | [ERROR] | Нэг данснаас өөр рүүгээ шилжүүлэхийг хориглоно. |
| Amount = Zero | [ERROR] | Дүн тэг буюу тэгээс бага бол шилжүүлэг хүчингүй. |
| Amount = Normal | [SINGLE] | Эерэг, үлдэгдэл болон лимитэд багтах дүнг төлөөлнө. |
| Amount = OverBalance | [ERROR] | Шилжүүлэх дүн үлдэгдлээс их байна. |
| Amount = OverLimit | [ERROR] | Шилжүүлэх дүн өдрийн лимитээс их байна. |
| Currency = Same | [SINGLE] | Валют хөрвүүлэлтгүй хэвийн шилжүүлгийг төлөөлнө. |
| Currency = Different | [IF] | Валют өөр үед хөрвүүлэлт хийж, шалгалтыг хөрвүүлсний дараах дүнгээр хийнэ. |

**Сонгосон алдааны давуу эрэмбэ:** ERROR_NO_FROM → ERROR_FROZEN → ERROR_NO_TO → ERROR_SAME_ACCOUNT → ERROR_BAD_AMOUNT → ERROR_INSUFFICIENT → ERROR_LIMIT.

Энэ эрэмбэ нь тодорхойлолтод заагаагүй олон алдааны үед тестийн хүлээгдэх үр дүнг нэг мөр болгох зорилготой таамаглал юм.

#### Хязгаарлалтын тооцоо

Бүрэн комбинацын тоо:

3 × 3 × 4 × 2 = 72.

From = Missing үед Amount = Normal гэж хязгаарлахад 12 комбинац хасагдана. To = Missing үед мөн 12 комбинац хасагдах боловч хоёр нөхцөл зэрэг биелэх 2 комбинацыг давхар хасахгүй.

Иймээс:

72 − 12 − 12 + 2 = 50.

**Бүрэн олонлог → хязгаарлалтын дараах олонлог: 72 → 50.**

Энэ тооцоо нь хоёр IF хязгаарлалтыг хэрэглэхэд үлдэх хүчинтэй комбинацын тоо юм. PICT-ийн pairwise гаралтын мөрийн тоо нь үүнээс өөр үзүүлэлт байна.

### A3. Спецификацийн хүснэгт

| № | From | To | Amount | Currency | Хүлээгдэх үр дүн |
|---:|---|---|---|---|---|
| 1 | Valid | Valid | Normal | Same | `201 {"result":"OK"}` |
| 2 | Missing | Valid | Normal | Same | `200 {"result":"ERROR_NO_FROM"}` |
| 3 | Frozen | Valid | Normal | Same | `200 {"result":"ERROR_FROZEN"}` |
| 4 | Valid | Missing | Normal | Same | `200 {"result":"ERROR_NO_TO"}` |
| 5 | Valid | SameAsFrom | Normal | Same | `200 {"result":"ERROR_SAME_ACCOUNT"}` |
| 6 | Valid | Valid | Zero | Same | `200 {"result":"ERROR_BAD_AMOUNT"}` |
| 7 | Valid | Valid | OverBalance | Same | `200 {"result":"ERROR_INSUFFICIENT"}` |
| 8 | Valid | Valid | OverLimit | Same | `200 {"result":"ERROR_LIMIT"}` |
| 9 | Valid | Valid | Normal | Different | `201 {"result":"OK"}` |
| 10 | Missing | Missing | Normal | Same | `200 {"result":"ERROR_NO_FROM"}` |
| 11 | Frozen | Valid | OverBalance | Different | `200 {"result":"ERROR_FROZEN"}` |
| 12 | Valid | SameAsFrom | Zero | Different | `200 {"result":"ERROR_SAME_ACCOUNT"}` |

Давхар нөхцөлийн үр дүнг сонгосон давуу эрэмбээр тодорхойлов. Хүлээгдэх үр дүн нь бодит хэрэгжилтээс баталгаажаагүй, спецификацид тулгуурласан тестийн таамаглал юм.
