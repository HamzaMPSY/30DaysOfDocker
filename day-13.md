# 30 Days of Docker - Day 13

<div dir="rtl">
    ايلا كنتي تاتخدم بdocker ضروري غادي تتعامل بزاف مع Shell خاصة في Dockerfiles .. اليوم غادي نشوفو واحدالمجموعة ديال القوالب و النصائح لي عندهم علاقة بShell و لي ممكن ينفعونا منين نكونو تانكتبو dockerfiles ديالنا
</div>

<div dir="ltr"><ul>
    <li>موما منين تايعيطو على شي program وسط الshell script ديالنا أو منين تانخدموا أي program في Linux .. تايطرى واحد البلان سميتو fork يعني أن kernel تايدير نسخة من process اﻷب (لي هو shell في الحالة ديالنا) و من بعد تايدير exec لي تاتشد الprogram ديالنا و تاتخشيه في هاديك النسخة الجديدة ديال الأب ..<br>
    طريقة أخرى باش نصايبو process جديد هي نحرقو المراحل و مانعيطوش على fork و ندوزو نيشان من exec ..يعني أننا غادي نشدو program ديالنا و غادي نخشيوه وسط الأب براسو.<br></li>
    أبسط مثال على هاذشي هو ps:<br>
    <ul>
</div>

    # here we'll get two processes the parent(shell) and child (ps)
    ps > processes.txt
    # Here we'll only get one process (ps). 
    exec ps > processes.txt

<div dir="ltr"><ul>
    هاذ اللعيبة (exec) لي هي shell wrapper over exec syscalls تاتفعنى منين تانخدمو Shell form في ENTRYPOINT أو CMD و تاتخلينا نبدلة process ديال الshell بالprocess ديال الprogram ديالنا و نتهناو من ديك الوصاية ديال shell لي هضرنا عليها في البوسط لي فات.

    <li>
    اللعيبة الثانية لي غادي نهضرو عليها هي set -euo pipefail .. من الحوايج الصعابين في shell scripting هو error handling .. شي مرات تاتكون الerror في الشرق و الميساج في الغرب ..هاذ الكومند غادي تعاونا نردو scripts ديالنا أفضل من ناحية error handling
    </li>
    <li>
    الe- : عموما واخا يكون سكريبت ديالنا فيه شي مشكل .. الshell تايكمل بحال ايلا ماكاين والو حتى الآخر .. هاذ e- تاتقول لshell يخرج ايلا شي كومند exit code ديالها ماكانش 0
    </li>
    <li>
    ال o pipefail- : ايلا كان الscript ديالنا فيه شي pipeline يعني هاذي | 😁 .. e- ماتانفعش بزاف بحيث أنها تاتشوف exit code ديال pipeline كاملة يعني ديال آخر كومند في pipeline فقط .. و هاذ pipefail تاتحل لينا هاذ المشكل و تاتبزز على shell يخرج ايلا لقى شي كومند في pipe ماخدماش..
    </li>
    <li>
    ال-u: هاذي عاوتاني مهمة بزاف .. عموما الshell ماعندوش مشكل مع empty variables باستعمال هاذ option ممكن نقولو ليه بي ايلا استعملنا شي variables خاوية را كاين مشكل.
    </li>
    <li>
    اللعيبة الثالثة هي Debugging ..ال set -x تاتقول لshell يكتب أي كومند قبل ما يexecute'ha هاكا تانعرفو فين كاين المشطل ايلا كان
    </li>

</ul></div>