# 30 Days of Docker - Day 24
السلام ديفسي الرباط
اليوم 24 #30daysofdocker
كيفاش Docker تايخزن container images ؟
باش نجاوبوا على السؤال غادي يخاسنا نختاروا شي container image ونجبدوها لعندنا.

    docker save node:alpine > node.tar
    skopeo docker://node:alpine docker-archive://node.tar

هاذشي غادي يعطينا واحد الفيشي tar لي فيه الimage ديالنا، هاذ الفيشي ممكن نديرو بيه لي بغينا بحال مثلا نسيفطوه في email لشي حد و ديك الساعة هة يimporteha عندو
دابا آجي نشوفو شنو فيه لداخل
    
    tar xvf node.tar

الmanifest.json و هو لي فيه معلومات عامة على image بحال فين كاينة config ديال الimage و الtags لي لاسقين مع image و سميات ديال layers لي كاينين و الترتيب ديالهم
الrepositories فيه معلومات على repository ديالنا و tags لي معاها
الconfig file تايكون جميع الconfigs ديال image بحال exposed ports و architecture و Workdir و user و CMD و بزاف ديال الحاجات آخرين لي تانزيدوهم في Dockerfile
الlayers: كل واحدة تاتكون في folder ديالها الخاص لي تايكون السمية ديالو هو الID ديال الlayer و لداخل فيه تيكون فيشيي سميتو json فيه المعلومات و الconfig الخاصة بديك layer و فيشي سميتو VERSION اللي فيه json schema version ديال هاذاك الفيشي json

الdocker image specification V1
هاذشي لي هذرنا عليه تايتسمى Docker image spec v1 و لي تايحدد كيفاش docker كان تايخزن container images في الأول .. دابا Docker داز ل Docker imager spec v2
من الحوايج لي فيهم مشكل في هاذ النسخة الأولى ديال specification هوما

    ماكاينينش قيود على كيفاش layer id خاص يكون it is an implementation detail، و عموما تايكون عدد عشوائي من 256bits و هاذشي كافي باش كل layer تكون عندها id ديالها مختلف على الآخرين .. و لكن ايلا تبدلات شي حاجة فيها ماعندناش شي طريقة باش نعرفوا بلي تبدلات و الimages لي تايعتامدوا عليها يقدر تطرا فيهم مشاكل regressions
    هاذ البلان تايتسمى content-addressablity
    كل layer عندها config file ديالها لي فيه شي حوايج بحال env و cmd و غيرهم، و لكن هاذشي ماعندو تا معنى مادام حنا ماكانستعملوش layers بوحدهم و لكن تانخدمو images
    منين بدا docker كان تايدعم x86_64 architectures و لكن مع الوقت تطورات الأمور و ولينا محتاجين الدعن ديال architectures آخرين و هاذشي ماكاين حتى شي طريقة باش نعبرو عليه باستعمال Docker image spec V1

هاذشي كامل خلا Docker تدوز لDocker image spec V2 لي تاتحل هاذ المشاكل .. و تاتزيد حوايج آخرين مهمين 

ال Docker image specification V2 فيها 2 نسخ:

- النسخة الأولى V2 Schema1 و لي تاتقدم واحد الطبقة ديال backward compatibility مع V1
- النسخة الثانية V2 Schema2 لي يحال V2S1 و لكن ماشي compatible مع V1 و أكثر تعبيرا .. هاذ القدرة التعبيرية تاتجي من Json Mediatypes الجداد لي تزادو و كيفاش تفرقات الأمور و ولات modular أكثر و من جهة أخرى ولات أغنى

أهم الحاجات اللي تبدلوا في V2S2 هوما
- الlayers ولاو files لي فيهم changeset فقط و مابقاش فيهم config خاصة و تاتولي عندنا config عامة ديال image فقط.

مابقاش عن باستعمال هاذ mediatypes الV2S2 تاتعطينا 2 دالحاجات مهمين:
- الmanifest.json ولا أكبر fat و أكثر تعبيرا .. عموما ولا فيه واحد list ديال manifests و كل واحد فيهم خاص بarchitecture معينة.
- كل manifest ولات فيه size و platform لي تاتجمع معلومات على architecture و os و غيرهم و digest لي تايكون فين hash value ديال config file.
و أخير الmanifest القديم ديال V1 تعوض بImage manifest و لي فيه معلومات على image و layers لي فيها و كل واحدة type ديالها، و size و digest ديال tar file لي تايطينا content addressablity لي دوينا عليها قبل.

على أساس هاذ الفورما V2S2 جات منظمة Open Container Initiative و دارت واحد Specification ديالها و لي تقريبا compatible مع V2S2 (مختلفين غير الأسماء و Image layout) .. و دابا ولا عندنا standard لي ممكن نتبعوه باش نصايبوا images ديالنا و يكونوا compatible مع أي OCI Compliant Container Engine.

- الصورة الأولى فيها الفرق بين oci image layout و docker v2s2
- الصورة الثانية فيها الفرق بين الmanifests لي كيفما تايبان لينا compatible مع بعضهم و الفرق غير في السميات ديال Mediatypes 😃