# 30 Days of Docker - Day 28
في لينكس كاينين 2 أنواع ديال الprocesses، الpriviliged processes و الunpriviliged processes. 
الpriviliged processes تايكونوا يقدروا يدروا لي بغاو و العكس بالنسية للpriviliged processes، 
يعني ايلا كتبنا شي بروغرام تايدير شي حوايج لي غير  root لي تايقددر يديرهم ماغاديش يقدر يديهم ايلا خدمناه بUnpriviliged user

هاذشي كان قبل النسخة 2.2 ديال لينكس .. من بعد ولاو الRoot priviliges مفرقين ماشي مجموعين، و هاذشي لي سميتو Capabilities.

أي Process ولا ممكن نعطيوه واحد العدد معين ديال Capabilities بلا مانعطيوه الPriviliges كاملين.
كاينين بزاف ديال Linux Capabilities منهم CAP_KILL لي كاتخلي Process يقدر يسيفط Signals ل processes آخرين. و CAP_SETUID و CAP_SETGID لي تايخلي الprocess يبدل المعلومات الخاصة ب user و group ديالو. 
آDocker تايخدم بزاف بLinux Capabilities باش يزير Containers 😆😆 و مايخليهمش يديروا لي بغاو. 
في Docker عندنا By default كل container تايجي بواحد الLinux capabilities و ممكن نزيدوا ليه شي واحدين و ننقصوا لي آخرين باستعمال cap-add و cap-drop