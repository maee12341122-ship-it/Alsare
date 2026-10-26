import 'package:flutter/material.dart';

void main() {
runApp(const TaizDeliveryApp());
}

class TaizDeliveryApp extends StatelessWidget {
const TaizDeliveryApp({super.key});

@override
Widget build(BuildContext context) {
return MaterialApp(
title: 'تطبيق توصيل تعز',
debugShowCheckedModeBanner: false,
theme: ThemeData(
primarySwatch: Colors.amber,
scaffoldBackgroundColor: const Color(0xFF121212),
brightness: Brightness.dark,
fontFamily: 'Cairo',
),
home: const TermsScreen(),
);
}
}

// ==========================================
// 1. شاشة الشروط والأحكام
// ==========================================
class TermsScreen extends StatefulWidget {
const TermsScreen({super.key});

@override
State<TermsScreen> createState() => _TermsScreenState();
}

class _TermsScreenState extends State<TermsScreen> {
bool _isAccepted = false;

@override
Widget build(BuildContext context) {
return Scaffold(
appBar: AppBar(
title: const Text('الشروط والأحكام'),
centerTitle: true,
backgroundColor: const Color(0xFF1F1F1F),
),
body: Padding(
padding: const EdgeInsets.all(20.0),
child: Column(
children: [
Expanded(
child: Container(
padding: const EdgeInsets.all(15),
decoration: BoxDecoration(
color: const Color(0xFF1F1F1F),
borderRadius: BorderRadius.circular(12),
border: Border.all(color: Colors.amber.withOpacity(0.3)),
),
child: SingleChildScrollView(
child: Directionality(
textDirection: TextDirection.rtl,
child: Column(
crossAxisAlignment: CrossAxisAlignment.start,
children: const [
Text(
'مرحباً بك في خدمة التوصيل - مدينة تعز',
style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold, color: Colors.amber),
),
SizedBox(height: 10),
Text(
'1. الالتزام بإدخال رقم هاتف يمني صحيح (مثل 77, 73, 71, 70) للتواصل عند التسليم.\n\n'
'2. يلتزم العميل بدفع قيمة الطلب ورسوم التوصيل عند الاستلام نقدًا أو عبر الحوالات المالية (الكريمي).\n\n'
'3. يحق للمشرف إلغاء أو تنسيق الطلب في حال تعذر التواصل مع العميل بعد عدة محاولات.\n\n'
'4. التوصيل متاح حالياً داخل نطاق مدينة تعز ومناطقها المحددة.',
style: TextStyle(fontSize: 14, height: 1.8),
),
],
),
),
),
),
),
const SizedBox(height: 15),
Row(
mainAxisAlignment: MainAxisAlignment.end,
children: [
const Text('أوافق على جميع الشروط والأحكام'),
Checkbox(
value: _isAccepted,
activeColor: Colors.amber,
onChanged: (val) {
setState(() {
_isAccepted = val ?? false;
});
},
),
],
),
const SizedBox(height: 15),
SizedBox(
width: double.infinity,
height: 50,
child: ElevatedButton(
style: ElevatedButton.styleFrom(
backgroundColor: Colors.amber,
foregroundColor: Colors.black,
shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(10)),
),
onPressed: _isAccepted
? () {
Navigator.pushReplacement(
context,
MaterialPageRoute(builder: (context) => const RegisterScreen()),
);
}
: null,
child: const Text('موافقة والمتابعة', style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold)),
),
),
],
),
),
);
}
}

// ==========================================
// 2. شاشة تسجيل بيانات العميل
// ==========================================
class RegisterScreen extends StatefulWidget {
const RegisterScreen({super.key});

@override
State<RegisterScreen> createState() => _RegisterScreenState();
}

class _RegisterScreenState extends State<RegisterScreen> {
final _formKey = GlobalKey<FormState>();
final TextEditingController _nameController = TextEditingController();
final TextEditingController _phoneController = TextEditingController();
final TextEditingController _addressController = TextEditingController();

String _selectedArea = 'شارع جمال';
final List<String> _taizAreas = [
'شارع جمال', 'الروضة', 'المسبح', 'عصيفرة', 'الحوبان', 'النشمة', 'بير باشا'
];

@override
Widget build(BuildContext context) {
return Scaffold(
appBar: AppBar(
title: const Text('تسجيل بيانات العميل'),
centerTitle: true,
backgroundColor: const Color(0xFF1F1F1F),
),
body: SingleChildScrollView(
padding: const EdgeInsets.all(20.0),
child: Directionality(
textDirection: TextDirection.rtl,
child: Form(
key: _formKey,
child: Column(
crossAxisAlignment: CrossAxisAlignment.start,
children: [
const Text('الرجاء إدخال بياناتك لتحديد موقع التوصيل بدقة في تعز', style: TextStyle(color: Colors.grey)),
const SizedBox(height: 20),

                // الاسم الكامل
                TextFormField(
                  controller: _nameController,
                  decoration: const InputDecoration(
                    labelText: 'الاسم الكامل',
                    border: OutlineInputBorder(),
                    prefixIcon: Icon(Icons.person, color: Colors.amber),
                  ),
                  validator: (val) => val == null || val.isEmpty ? 'يرجى إدخال الاسم' : null,
                ),
                const SizedBox(height: 15),

                // الرقم اليمني
                TextFormField(
                  controller: _phoneController,
                  keyboardType: TextInputType.phone,
                  decoration: const InputDecoration(
                    labelText: 'رقم الهاتف اليمني (77 / 73 / 71 / 70)',
                    border: OutlineInputBorder(),
                    prefixIcon: Icon(Icons.phone, color: Colors.amber),
                  ),
                  validator: (val) {
                    if (val == null || val.isEmpty) return 'يرجى إدخال رقم الهاتف';
                    final regExp = RegExp(r'^(77|73|71|70|78)\d{7}$');
                    if (!regExp.hasMatch(val)) return 'رقم يمني غير صحيح (يجب أن يبدأ بـ 77, 73, 71, 70 ومكون من 9 أرقام)';
                    return null;
                  },
                ),
                const SizedBox(height: 15),

                // المنطقة
                DropdownButtonFormField<String>(
                  value: _selectedArea,
                  decoration: const InputDecoration(
                    labelText: 'المنطقة في تعز',
                    border: OutlineInputBorder(),
                    prefixIcon: Icon(Icons.location_city, color: Colors.amber),
                  ),
                  items: _taizAreas.map((area) {
                    return DropdownMenuItem(value: area, child: Text(area));
                  }).toList(),
                  onChanged: (val) => setState(() => _selectedArea = val!),
                ),
                const SizedBox(height: 15),

                // العنوان التفصيلي
                TextFormField(
                  controller: _addressController,
                  maxLines: 2,
                  decoration: const InputDecoration(
                    labelText: 'عنوان البيت / المحل / علامة مميزة',
                    border: OutlineInputBorder(),
                    prefixIcon: Icon(Icons.home, color: Colors.amber),
                  ),
                  validator: (val) => val == null || val.isEmpty ? 'يرجى تحديد العنوان التفصيلي' : null,
                ),
                const SizedBox(height: 25),

                SizedBox(
                  width: double.infinity,
                  height: 50,
                  child: ElevatedButton(
                    style: ElevatedButton.styleFrom(backgroundColor: Colors.amber, foregroundColor: Colors.black),
                    onPressed: () {
                      if (_formKey.currentState!.validate()) {
                        Navigator.pushReplacement(
                          context,
                          MaterialPageRoute(
                            builder: (context) => HomeScreen(
                              userName: _nameController.text,
                              userPhone: _phoneController.text,
                              userAddress: '$_selectedArea - ${_addressController.text}',
                            ),
                          ),
                        );
                      }
                    },
                    child: const Text('دخول للتطبيق', style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold)),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
}
}

// ==========================================
// 3. الشاشة الرئيسية والاقسام الأربعة
// ==========================================
class HomeScreen extends StatefulWidget {
final String userName;
final String userPhone;
final String userAddress;

const HomeScreen({
super.key,
required this.userName,
required this.userPhone,
required this.userAddress,
});

@override
State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
int _selectedCategoryIndex = 0;

final List<Map<String, dynamic>> _restaurants = [
{'name': 'مطعم الشيباني', 'item': 'وجبة مشكل سفري', 'price': '3500 ر.ي'},
{'name': 'مطعم الشرق', 'item': 'دجاج شواية مع الأرز', 'price': '4000 ر.ي'},
{'name': 'مطعم السعيد', 'item': 'سلتة تعزية باللحم', 'price': '2500 ر.ي'},
];

final List<Map<String, dynamic>> _groceries = [
{'name': 'بقالة النور', 'item': 'حليب ممتاز + خبز', 'price': '1200 ر.ي'},
{'name': 'سوبرماركت المدينة', 'item': 'كرتون ماء تعز', 'price': '1800 ر.ي'},
{'name': 'مكتبة ومواد', 'item': 'أرز بسمتي 5 كيلو', 'price': '8500 ر.ي'},
];

final List<Map<String, dynamic>> _gifts = [
{'name': 'متجر العطور', 'item': 'عطر بخور يمني ملكي', 'price': '5000 ر.ي'},
{'name': 'ساعات تعز', 'item': 'ساعة يد رجالي أنيقة', 'price': '12000 ر.ي'},
{'name': 'هدايا وتغليف', 'item': 'طقم هدايا نسائي متكامل', 'price': '15000 ر.ي'},
];

@override
Widget build(BuildContext context) {
return Directionality(
textDirection: TextDirection.rtl,
child: Scaffold(
appBar: AppBar(
backgroundColor: const Color(0xFF1F1F1F),
title: Text('أهلاً بك، ${widget.userName}'),
actions: [
IconButton(
icon: const Icon(Icons.admin_panel_settings, color: Colors.amber),
tooltip: 'لوحة المشرف',
onPressed: () {
Navigator.push(
context,
MaterialPageRoute(builder: (context) => const AdminDashboard()),
);
},
)
],
),
body: Column(
children: [
// شريط البحث
Padding(
padding: const EdgeInsets.all(12.0),
child: TextField(
decoration: InputDecoration(
hintText: 'ابحث عن مطعم، بقالة، أو منتج...',
prefixIcon: const Icon(Icons.search, color: Colors.amber),
filled: true,
fillColor: const Color(0xFF1F1F1F),
border: OutlineInputBorder(borderRadius: BorderRadius.circular(10), borderSide: BorderSide.none),
),
),
),

            // الأقسام الأربعة
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceAround,
              children: [
                _buildCategoryTab(0, 'المطاعم', Icons.restaurant),
                _buildCategoryTab(1, 'البقالات', Icons.local_grocery_store),
                _buildCategoryTab(2, 'هدايا', Icons.card_giftcard),
                _buildCategoryTab(3, 'طلبات أخرى', Icons.edit_note),
              ],
            ),
            const Divider(color: Colors.amber),

            // المحتوى المعروض حسب القسم
            Expanded(
              child: _buildCategoryContent(),
            ),
          ],
        ),
      ),
    );
}

Widget _buildCategoryTab(int index, String title, IconData icon) {
bool isSelected = _selectedCategoryIndex == index;
return GestureDetector(
onTap: () => setState(() => _selectedCategoryIndex = index),
child: Container(
padding: const EdgeInsets.symmetric(vertical: 8, horizontal: 12),
decoration: BoxDecoration(
color: isSelected ? Colors.amber : const Color(0xFF1F1F1F),
borderRadius: BorderRadius.circular(8),
),
child: Column(
children: [
Icon(icon, color: isSelected ? Colors.black : Colors.amber),
const SizedBox(height: 4),
Text(title, style: TextStyle(color: isSelected ? Colors.black : Colors.white, fontWeight: FontWeight.bold, fontSize: 12)),
],
),
),
);
}

Widget _buildCategoryContent() {
if (_selectedCategoryIndex == 3) {
return CustomOrderScreen(
userName: widget.userName,
userPhone: widget.userPhone,
userAddress: widget.userAddress,
);
}

    List items = _selectedCategoryIndex == 0
        ? _restaurants
        : (_selectedCategoryIndex == 1 ? _groceries : _gifts);

    return ListView.builder(
      itemCount: items.length,
      itemBuilder: (context, idx) {
        var item = items[idx];
        return Card(
          color: const Color(0xFF1F1F1F),
          margin: const EdgeInsets.symmetric(horizontal: 12, vertical: 6),
          child: ListTile(
            title: Text(item['name'], style: const TextStyle(fontWeight: FontWeight.bold)),
            subtitle: Text(item['item']),
            trailing: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              crossAxisAlignment: CrossAxisAlignment.end,
              children: [
                Text(item['price'], style: const TextStyle(color: Colors.amber, fontWeight: FontWeight.bold)),
                const SizedBox(height: 4),
                InkWell(
                  onTap: () {
                    ScaffoldMessenger.of(context).showSnackBar(
                      SnackBar(content: Text('تمت إضافة (${item['item']}) إلى السلة!')),
                    );
                  },
                  child: const Text('إضافة للسلة', style: TextStyle(fontSize: 11, color: Colors.green, fontWeight: FontWeight.bold)),
                ),
              ],
            ),
          ),
        );
      },
    );
}
}

// ==========================================
// 4. شاشة طلبات أخرى (الطلب الخاص)
// ==========================================
class CustomOrderScreen extends StatefulWidget {
final String userName;
final String userPhone;
final String userAddress;

const CustomOrderScreen({
super.key,
required this.userName,
required this.userPhone,
required this.userAddress,
});

@override
State<CustomOrderScreen> createState() => _CustomOrderScreenState();
}

class _CustomOrderScreenState extends State<CustomOrderScreen> {
final TextEditingController _customDetailsController = TextEditingController();

@override
Widget build(BuildContext context) {
return Padding(
padding: const EdgeInsets.all(16.0),
child: Column(
crossAxisAlignment: CrossAxisAlignment.start,
children: [
const Text('كتابة طلب خاص (أدوية، غاز، أغراض أخرى)', style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold, color: Colors.amber)),
const SizedBox(height: 10),
const Text('اكتب تفاصيل طلبك بدقة وسيقوم المشرف بالتنسيق مع المندوب لشرائه وإيصاله إليك.', style: TextStyle(color: Colors.grey, fontSize: 13)),
const SizedBox(height: 15),
TextField(
controller: _customDetailsController,
maxLines: 4,
decoration: const InputDecoration(
hintText: 'مثال: أسطوانة غاز من توزع صيرة، أو علاج معين من الصيدلية...',
border: OutlineInputBorder(),
filled: true,
fillColor: Color(0xFF1F1F1F),
),
),
const SizedBox(height: 20),
SizedBox(
width: double.infinity,
height: 48,
child: ElevatedButton.icon(
style: ElevatedButton.styleFrom(backgroundColor: Colors.amber, foregroundColor: Colors.black),
icon: const Icon(Icons.send),
label: const Text('إرسال الطلب الخاص للمشرف'),
onPressed: () {
if (_customDetailsController.text.isNotEmpty) {
ScaffoldMessenger.of(context).showSnackBar(
const SnackBar(content: Text('تم إرسال طلبك الخاص للمشرف بنجاح! سيتم التواصل معك.')),
);
_customDetailsController.clear();
} else {
ScaffoldMessenger.of(context).showSnackBar(
const SnackBar(content: Text('الرجاء كتابة تفاصيل الطلب أولاً')),
);
}
},
),
)
],
),
);
}
}

// ==========================================
// 5. لوحة تحكم المشرف (Admin Dashboard)
// ==========================================
class AdminDashboard extends StatefulWidget {
const AdminDashboard({super.key});

@override
State<AdminDashboard> createState() => _AdminDashboardState();
}

class _AdminDashboardState extends State<AdminDashboard> {
final List<Map<String, dynamic>> _orders = [
{
'id': '101',
'client': 'معين هشام',
'phone': '771234567',
'address': 'شارع جمال - بجانب المجمع',
'items': 'مطعم الشيباني: وجبة مشكل سفري',
'status': 'قيد الانتظار',
'driver': 'لم يتم التعيين بعد'
},
{
'id': '102',
'client': 'أحمد علي',
'phone': '739876543',
'address': 'الروضة - قرب المدرسة',
'items': 'طلب خاص: علاج من صيدلية الحكمة',
'status': 'جاري التوصيل',
'driver': 'المندوب: طارق (77xxxxxxx)'
}
];

@override
Widget build(BuildContext context) {
return Directionality(
textDirection: TextDirection.rtl,
child: Scaffold(
appBar: AppBar(
title: const Text('لوحة تحكم المشرف - إدارة الطلبات'),
backgroundColor: const Color(0xFF1F1F1F),
),
body: ListView.builder(
itemCount: _orders.length,
itemBuilder: (context, idx) {
var order = _orders[idx];
return Card(
color: const Color(0xFF1F1F1F),
margin: const EdgeInsets.all(10),
child: Padding(
padding: const EdgeInsets.all(12.0),
child: Column(
crossAxisAlignment: CrossAxisAlignment.start,
children: [
Row(
mainAxisAlignment: MainAxisAlignment.spaceBetween,
children: [
Text('طلب #${order['id']} - ${order['client']}', style: const TextStyle(fontWeight: FontWeight.bold, fontSize: 16)),
Chip(
label: Text(order['status']),
backgroundColor: order['status'] == 'قيد الانتظار' ? Colors.orange : Colors.green,
)
],
),
const SizedBox(height: 5),
Text('رقم العميل: ${order['phone']}', style: const TextStyle(color: Colors.amber)),
Text('العنوان في تعز: ${order['address']}', style: const TextStyle(color: Colors.grey)),
const SizedBox(height: 8),
Text('تفاصيل الطلب: ${order['items']}', style: const TextStyle(color: Colors.white)),
const SizedBox(height: 5),
Text('الحالة والمندوب: ${order['driver']}', style: const TextStyle(color: Colors.cyan, fontSize: 12)),
const Divider(),
Row(
mainAxisAlignment: MainAxisAlignment.spaceBetween,
children: [
TextButton.icon(
icon: const Icon(Icons.phone, size: 16, color: Colors.green),
label: const Text('اتصال بالعميل', style: TextStyle(fontSize: 12)),
onPressed: () {},
),
ElevatedButton.icon(
style: ElevatedButton.styleFrom(backgroundColor: Colors.amber, foregroundColor: Colors.black),
icon: const Icon(Icons.delivery_dining, size: 18),
label: const Text('تنسيق وإسناد لمندوب'),
onPressed: () {
setState(() {
order['status'] = 'جاري التوصيل';
order['driver'] = 'المندوب: خليل (تم التواصل للشراء)';
});
},
)
],
)
],
),
),
);
},
),
),
);
}
}