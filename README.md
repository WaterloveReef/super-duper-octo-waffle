# super-duper-octo-waffle
#include <iostream>

using namespace std;
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>
#include<unistd.h>
#include <stdbool.h>

#define MAX_NAME_LENGTH 20// 最长名字长度
#define MAX_DESCRIPTION_LENGTH 50
#define MAX_CONDITION_LENGTH 50
#define MAX_INPUT_LENGTH 20
int numOfNPC=0;
int health_max = 50;
int health = 50;
int attack = 0;
int attack0 = 0;
int enemy_health = 50;
int enemy_health_max = 50;
int enemy_attack = 10;
int choice4;
int damage;
int enemy_damage;
int flag = 0;
int counter=0;
int i=0;
int answer = 0; 
int heal;
int enemy_heal;
int location = 0; // 0表示城镇，1表示荒郊，2表示神秘的城堡
int experience = 0;
int enemy_experience = 0;
int level = 1;
int enemy_level = 1;
int level_up_experience = 100; //经验和等级
int weapon = 0;
int townlocation = 0;
int classroom1location = 0;
char name0[10];
int status = 0;
int back = 0;
int luck = 0;
int Luck = 80;
int dialogue = 0;
int year = 1;
int month = 1;
int week = 1;
int day = 1;
int hour = 8;
int minute=0;
int record=0;
int curiosity=50;
int morality=50;
int mainlocation;

int neigong_level=0;
int waigong_level=0;
int qinggong_level=0;

int gengu=60;
int bili=60;
int shenfa=60;
int jingjie=0;
int energy=0;
int contribution=0;
int neigongexperience=0;
int waigongexperience=0;
int qinggongexperience=0;
int bili0;
int jingjie0;

int switches[99];
int quit;
int turn,choice;
int playerdamage,enemydamage;
int choice1,choice2,choice3;
typedef struct {
    char npcname[MAX_NAME_LENGTH];
    int HP;
    int HPMAX;
    int attack;
    int dodge;
    int morality;
    bool live;
}NPC;
NPC npc[1000];
NPC player={"主角",200,200,40,5,50,true};
void fight(NPC *player, NPC *enemy) {
    int player_damage, enemy_damage;
    int player_miss, enemy_miss;
    turn = 1;
    flag=0;
    playerdamage = 0;
    enemydamage = 0;
    enemy-> HP = enemy-> HPMAX;
    while (player-> HP > 0 && enemy-> HP > 0) {
        printf("==== 回合 %d ==== \n", turn);
        
        // Player's turn
        printf("%s 攻击 %s. \n", player-> npcname, enemy-> npcname);
        player_miss = rand() % 100;
        if (player_miss > enemy-> dodge) {
            player_damage = rand() % (player-> attack /2)+(player-> attack /2)+ 1; // attack value plus one to avoid 0 damage
            enemy-> HP -= player_damage;
            playerdamage+= player_damage;
            printf("%s 受到了 %d 点伤害。 \n", enemy-> npcname, player_damage);
        } else {
            printf("%s 躲开了你的攻击。 \n", enemy-> npcname);
        }
        
        if (enemy-> HP <= 0) {
            printf("%s 击败了 %s!取得了胜利！ \n", player-> npcname, enemy-> npcname);
            flag=1;
            break;
        }
        
        // Enemy's turn
        printf("%s 攻击 %s. \n", enemy-> npcname, player-> npcname);
        enemy_miss = rand() % 100;
        if (enemy_miss > player-> dodge) {
            enemy_damage = rand() % (enemy-> attack /2)+(enemy-> attack)/2 + 1;
            //enemy_damage -= rand() % (gengu/10);
            player-> HP -= enemy_damage;
            enemydamage+= enemy_damage;
            printf("%s 受到了 %d 点伤害。 \n", player-> npcname, enemy_damage);
        } 
        else {
            printf("%s 躲开了这次攻击。 \n", player-> npcname);
        }
        if (player-> HP <= 0) {
            printf("%s 被击败了! \n", player-> npcname);
            if (player-> HP <= (0-player-> HPMAX)/2) {
                player-> HP = (0-player-> HPMAX)/2;
                enemydamage = 2*player-> HPMAX;
            }
            break;
        }
        turn++;
        printf("你剩余的生命值%d\n", player-> HP);
        printf("%s剩余的生命值%d\n", enemy-> npcname, enemy-> HP);
        sleep(1);
    }
    
}

const char *surnames[] = {"赵", "钱", "孙", "李", "周", "吴", "郑", "王", "冯", "陈", "褚", "卫", "蒋", "沈", "韩", "杨", "朱", "秦", "尤", "许", "何", "吕", "施", "张", "孔", "曹", "严", "华", "金", "魏", "陶","姜", "戚", "谢", "邹", "喻", "柏", "水", "窦", "章", "云", "苏", "潘", "葛", "奚", "范", "彭", "郎", "鲁", "韦", "昌", "马", "苗", "凤", "花", "方", "俞", "任", "袁", "柳", "酆", "鲍", "史", "唐", "费", "廉", "岑", "薛", "雷", "贺", "倪", "汤", "滕", "殷", "罗", "毕", "郝", "邬", "安", "常", "乐","于", "时", "傅", "皮", "卞", "齐", "康", "伍", "余", "元", "卜", "顾", "孟", "平", "黄", "和", "穆", "萧", "尹", "姚", "邵", "湛", "汪", "祁", "毛", "禹", "狄", "米", "贝", "明", "臧", "计", "伏", "成", "戴", "谈", "宋", "茅", "庞", "熊", "纪", "舒", "屈", "项", "祝", "董", "梁","高", "夏", "蔡", "田", "樊", "胡", "凌", "霍", "万", "柯", "卢", "郝", "丁", "钱", "房", "崔", "蒲", "关", "龚", "江", "邱", "皮", "骆", "凤", "段", "裴", "缪", "宁", "段", "柴", "檀", "满", "乔", "蓝", "屠", "龙", "殳", "钟", "申", "贾", "娄", "邹", "叶", "萧", "尚", "堵", "储", "靳", "汲", "邴", "邝", "糜", "杭", "卫", "冼", "钦", "司徒","西门", "左丘", "东方", "欧阳", "南宫", "西门", "司马", "公输", "轩辕", "令狐","段干", "百里", "东郭", "南门", "呼延", "羊舌", "微生", "梁丘", "左史", "公冶", "宗政", "濮阳", "长孙", "司空", "鞠绰", "种姓", "赫连", "司寇", "上官", "皇甫", "公孙", "仲孙"};
const char *names[] = {"美", "泉", "琳","茜","雯", "莎", "晨", "梅", "晶", "萱", "露", "媛","琼", "楠", "婷", "诗", "茗", "卉", "嘉", "梦", "艳", "蓓", "慧", "丽","欣", "红", "妮", "菲", "娜", "静", "颖", "帅", "钰", "芝","燕","誉","钗","狐","凰","儿","辕","影","墨","剑","楼","死","情","门","凤","玄","衣","开","花","月","名","蓉","复","拯","女","敏","云","雕","易","儿","伟","勇","富","强","华","俊","龙","军","宇","超","志","林","坤","杰","鹏","波","斌","浩","博","旭","鑫","利","昌","鸿","涛","文","健","航","东","云","磊","民","庆","宁","欣","川","亮","弘","达","洋","严","益","永","朋","江","升","易","峰","莉","春","清","明","中","鸣","柏","发","源","帅","毅","记","枫","锐","紫衣","留香","晓生","名权","铁心","不悔","小凤","紫杉","静璇","鼎公","公子","小龙","少华","三丰","语嫣","不群","盈盈","世玉","震子","白衣","子龙","志丙","建国","开元","明珠","芳华","闲云","非烟","若兰","青竹","天明","如风","倩影","瑶瑾","丽娜","思琳","玲儿","佩文","琳琅","正豪","明月","文清","文博","若溪","翠姝","紫菱","梅花","雁婷","彦坤","晓青","怡君","秋蕙","雨涵","卉儿","颜芊","思宁","旭东","君雅","世峰","春华","俊杰","梦婷","星宇","运营","姝婳","思妍","典范","倩莹","卓越","涓涓","夏阳","文彦","恬静","梦露","嘉骏","辰阳","光辉","思嘉","佳欣","晋豫","佳莹","茹婷","妍华","秀娟","雨萌","丹琳","涵韵","梓豪","彦霖","浩天","睿婷","若彤","慕婷","秀兰","飞虎","晓玲","洞宾","十一郎"};
    int s_len = sizeof(surnames) / sizeof(surnames[0]);  // 姓氏的数量
    int n_len = sizeof(names) / sizeof(names[0]);  // 名字的数量
    char name[MAX_NAME_LENGTH][100];  // 定义一个字符串来保存名字
    int j=0;
    int m[50];
    int a,b,c,d,e,f,g,h;
 void generatenpc(){
    int i=0;

    for (i = 0; i < 40; i++) {  // 随机生成10个名字
    
    // 随机选取一个姓氏
    int s_index = rand() % s_len;
   const char *surname = surnames[s_index];
    
    // 随机选取一个名字
    int n_index = rand() % n_len;
    const char *given_name = names[n_index];
    
    // 将姓氏和名字组合成一个完整的名字
    snprintf(name[numOfNPC], MAX_NAME_LENGTH, "%s%s", surname, given_name);
    
    // 输出名字
    npc[numOfNPC].HP=100+2*i;
    npc[numOfNPC].HPMAX=100+2*i;
    npc[numOfNPC].attack=20+i*2/5;
    npc[numOfNPC].dodge=rand()%5;
    npc[numOfNPC].morality=40+rand()%11+rand()%11;
    npc[numOfNPC].live=true;
    strcpy(npc[numOfNPC].npcname,name[numOfNPC]);
    printf(" %d.  %s\n", numOfNPC+1,npc[numOfNPC].npcname);
    numOfNPC++;

}
}
void liangong(){
    printf("你获得了%d点经验！\n", (enemydamage+playerdamage)*turn/10);
    flag = 0;
    if (waigong_level < 10&&waigong_level > 0) {
        waigongexperience+= (enemydamage+playerdamage)*turn/10;
    }
    if (neigong_level < 10&&neigong_level > 0) {
        neigongexperience+= (enemydamage+playerdamage)*turn/10;
    }
    if (qinggong_level < 10&&qinggong_level > 0) {
        qinggongexperience+= (enemydamage+playerdamage)*turn/10;
    }
    while (waigongexperience >= waigong_level*100&&waigong_level < 10&&waigong_level > 0)
    {  waigongexperience-= waigong_level*100;
        waigong_level++;
        flag = 1;
    }
    if (flag == 1) {
        printf("恭喜你基本外功等级提升了，当前等级为%d级！\n", waigong_level);
        flag = 0;
    }
    while (neigongexperience >= neigong_level*100&&neigong_level < 10&&neigong_level > 0)
    {   neigongexperience-= neigong_level*100;
        neigong_level++;
        gengu++;
        flag = 1;
    }
    if (flag == 1) {
        printf("恭喜你基本内功等级提升了，当前等级为%d级！当前根骨为%d。\n", neigong_level, gengu);
        flag = 0;
    }
    while (qinggongexperience>=qinggong_level*100&&qinggong_level < 10&&qinggong_level > 0)
    {   qinggongexperience-= qinggong_level*100;
        qinggong_level++;
        shenfa++;
        flag = 1;
    }
    if (flag == 1) {
        printf("恭喜你基本轻功等级提升了，当前等级为%d级！当前身法为%d。\n", qinggong_level, shenfa);
        flag = 0;
    }
    if (waigong_level >= 10) {
        printf("你的基本外功似乎已经到了一个极限，暂时无法再提升了。\n");
    }
    if (neigong_level >= 10) {
        printf("你的基本内功似乎已经到了一个极限，暂时无法再提升了。\n");
    }
    if (qinggong_level >= 10) {
        printf("你的基本轻功似乎已经到了一个极限，暂时无法再提升了。\n");
    }
}
//这是一个能够随机产生NPC并能实现与玩家交互的初步的功能，npc()是一个结构体数组，储存了大量的NPC数据，请你进一步完善这个功能，使它能够成为一个完善的功能。
void printnpc(){
a=rand()%20;

int i,j,n;
for(i=0;i<a;i++){
m[i]=rand()%40;
for (int j = 0; j < i; j++) {
if (m[j] == m[i]||npc[m[i]].live==false) { // 如果生成的数已存在，则重新生成
i--;
if(npc[m[i]].live==false){
    a--;
}
break;
}
}
}
if(a==0){
    printf("你的附近空无一人。\n");
    return;
}
while(1){
cout<<"你附近有这些人：\n";
for(i=0;i<a;i++){
if(npc[m[i]].live==true){
cout<<i+1<<". "<<npc[m[i]].npcname<<endl;
}
if (npc[m[i]].live==false) { // 如果NPC以死亡，则不会生成
for(n=i;n<a-1;n++){
m[n]=m[n+1];
}
i--;
a--;
}

}
if(a==0){
    printf("你的附近空无一人。\n");
    return;
}
cout<<"请选取一个你要进行交互的人，输入"<<i+1<<"离开。"<<endl;
cin>>choice;
choice=choice-1;
if(choice==i){
    choice=0;
    choice1=0;
    return;
}
while(choice>=0&&choice<i){
if(npc[m[choice]].live==true){
cout<<"你来到了"<<npc[m[choice]].npcname<<"面前。"<<endl;
cout<<"你可以：\n"<<"1. 交谈\n2. 切磋\n3. 攻击\n4. 离开"<<endl;    
cin>>choice1;
if(choice1>0&&choice1<=4){
if(choice1==1){
cout<<"你好。\n";
    
}
if(choice1==2){
fight(&player,&npc[m[choice]]);
cout<<"你们点到即止，结束了切磋。\n";
player.HP=player.HPMAX;
liangong();
}    
if(choice1==3){
fight(&player,&npc[m[choice]]);
while(flag==1){
    printf("你击败了%s!你可以：\n",npc[m[choice]].npcname);
    printf("1. 杀死%s\n",npc[m[choice]].npcname);
    printf("2. 放过%s\n",npc[m[choice]].npcname);
    scanf("%d",&choice2);
    if(choice2==1){
       printf("%s被你杀死了！\n",npc[m[choice]].npcname); 
       npc[m[choice]].live=false;
       flag=0;
       if(npc[m[choice]].morality>=45&&player.morality>35){
           player.morality-=1;
           printf("你的道德值下降了！当前道德值为%d\n",player.morality); 
       }
       if(npc[m[choice]].morality<45&&player.morality<65){
           player.morality+=1;
           printf("你的道德值增加了！当前道德值为%d\n",player.morality); 
       }
    }
    if(choice2==2){
       printf("你选择放过%s。\n",npc[m[choice]].npcname); 
       flag=0;
    }
}
}    
if(choice1==4){
choice=0;
break;
}   
}
}
else{
    choice=0;
    choice1=0;
  break;
}
}
}
}


 void Time(int x, int y){
        minute+= y;
        hour = hour+x;
        record+= x;
        while (minute >= 60) {
            hour++;
            record++;
            minute-= 60;
        }
        minute = minute % 60;
        while (hour >= 24) {
            day++;
            switches[2] = 1;
            hour-= 24;
        }
        hour = hour % 24;
        while (day >= 29){
            flag = 0;
            if (month == 4||month == 6||month == 9||month == 11) {
                if (day >= 31) {
                    month++;
                    day-= 30;
                    flag = 1;
                }
            }
            else if (month == 2) {
                if (day >= 29) {
                    month++;
                    day-= 28;
                    flag = 1;
                }
            }
            else {
                if (day >= 32) {
                    month++;
                    day-= 31;
                    flag = 1;
                }
            }
            if (flag == 0) {
                break; 
            }
        }
        while (month >= 13) {
            year++;
            month-= 12;
        }
        month = month % 13;
        printf("现在是姆%d年%d月%d日%d时%d分。\n", year+1000, month, day, hour, minute); 
    }
    

int main(){
    srand(time(NULL));
for (i = 0; i < 98; i++) {
        switches[i] = 1;
    }
location = 4;
    record = 0;
    i=0;
    generatenpc();
    while (location == 4&& record <= 720)
    { 
        
        if (classroom1location == 0) // 教室
        {   
            if (hour <= 5) {
                player.HP-= 10;
                printf("你正在熬夜，熬夜时进行任何活动都会扣除健康值。\n");
                printf("你当前的健康值为%d点，请尽快返回寝室休息！\n", player.HP);
            }
            turn = 0;
            printf("你现在在教室，你可以：\n");
            printf("1. 和小队长交谈，获取关于魔法和这个世界的信息\n");
            printf("2. 尝试归引同化魔法能量到自己体内\n");
            printf("3. 向小队长学习基础魔法\n");
            printf("4. 前往比武擂台\n");
            printf("5. 去寝室休息\n");
            printf("6. 砍树劈柴\n");
            printf("7. 前往空地\n");
            printf("8. 挑水\n");
            printf("9. 查看附近的人\n");
            printf("请输入你的选择：");
            scanf("%d", &choice);
            if(choice==9){
                printnpc();
                
            }
            if (choice == 1)
            {   dialogue = rand() % 8;
                printf("*\n");
                if (dialogue == 0) {
                    printf("小队长:我每天下午会在教室中央传授魔法。\n");
                }
                if (dialogue == 1) {
                    printf("小队长:在擂台上可以对战木头人和其他人，积累实战经验。\n");
                }
                if (dialogue == 2) {
                    printf("小队长:想锻炼身法可以去空地。\n");
                }
                if (dialogue == 3) {
                    printf("小队长:砍树劈柴有利于改善根骨。\n");
                }
                if (dialogue == 4) {
                    printf("小队长:挑水可以锻炼臂力。\n");
                }
                if (dialogue == 5) {
                    printf("小队长:不要浪费时间，抓紧机会提升自己。\n");
                }
                if (dialogue == 6) {
                    printf("小队长:先学习魔法，再通过实战来提升是最优的训练方式。\n");
                }
                if (dialogue == 7) {
                    printf("小队长:等我突破了这一重境界就可以晋升为正式小队长了。\n");
                }
                printf("*\n");
                while (choice == 1) {
                    printf("你想了解什么？\n");
                    printf("1. 什么是魔法？\n");
                    printf("2. 为什么要学习魔法？\n");
                    printf("3. 魔法学习和修炼分为几个阶段？\n");
                    printf("4. 应该如何学习魔法？\n");
                    printf("5. 魔法修炼的修为分为几个境界？\n");
                    printf("6. 这个世界是怎么样的？\n");
                    printf("7. 英雄协会和我们协会的地理和战争形势是怎么样的？\n");
                    printf("8. 我们勇者协会附近有什么？\n");
                    printf("9. 我可以和你单挑吗？\n");
                    printf("10. 离开\n");
                    printf("请输入你的选择：");
                    scanf("%d", &choice1);
                    printf("*\n");
                    if (choice1 == 1) {
                        printf("小队长:魔法是物质对世界中的能量运用的形式，是人类用来认知世界、改造世界的方式。\n" );
                        turn++;
                    }
                    if (choice1 == 2) {
                        printf("小队长:魔法是物质对世界中的能量运用的形式，是人类用来认知世界、改造世界的方式。魔法的发现彻底改变了人类社会的社会结构、经济模式、生活方式，给人类社会的方方面面都带来了深远的影响，对于我们这个乱世来说，不学习魔法就无法自保，我们也需要强大的魔法师来结束这场战争，保卫天下百姓。\n" );
                        turn++;
                    }
                    if (choice1 == 3) {
                        printf("小队长:学习模仿、刻苦修炼、实战练习、归纳总结、运用自如、创造出符合自己的魔法、最终达到天人合一的境界。\n" );
                        turn++;
                    }
                    if (choice1 == 4) {
                        printf("小队长:首先要打好基本功，练好基础魔法，然后刻苦修炼、外出游历、在战场上进行实战，多听多看，保持好奇心与求知欲，有一颗探索和冒险的心是关键。\n" );
                        turn++;
                    }
                    if (choice1 == 5) {
                        printf("小队长:魔法修炼的境界分为G、F、E、D、C、B、A七个境界，你们目前都是G级魔法师，也就是刚刚成为魔法师。\n" );
                        turn++;
                    }
                    if (choice1 == 6) {
                        printf("小队长:我们生活在宇宙中的一颗球形星球唯一的一块陆地上，没错就是姆大陆。\n" );
                        turn++;
                    }
                    if (choice1 == 7) {
                        printf("小队长:英雄协会割据了人族的另外一半领地，我们和英雄协会在北方的正面战场已经因为常年的战争而变得寸草不生，一般情况下根本不会有人或其他生物去那里，现在我们和英雄协会的主战场是侧面西北方的两个战场，那里地形更加复杂，双方僵持已久。\n" );
                        turn++;
                    }
                    if (choice1 == 8) {
                        printf("小队长:北方是一片荒凉的正面战场，越过这片战场就能到达英雄协会的领地;西北方向是当今的两处主战场;东方是植物文明的领地;东北方向是塑胶群落，那里危险与机遇并存，是一处游历历练的好地方;东南方向是黑暗森林，那里十分混乱，怪物横行，非常危险;南方是我们人族的后方领地。\n" );
                        turn++;
                    }
                    if (choice1 == 9) {
                        printf("小队长:等你出去了就知道我们勇者协会强者如云了。\n" );
                        turn++;
                    }
                    if (choice1 == 10) {
                        printf("你选择离开。\n" );
                        Time(0, turn+1);
                        choice = 0;
                    }
                    printf("*\n");
                }
            }
            else if (choice == 3)
            {
                printf("你打算向小队长学习一些基础魔法。\n");
                if (hour < 18&&hour > 12) {
                    printf("小队长正在教室中央讲解一些基础魔法。\n");
                    dialogue = rand() % 3;
                    if (dialogue == 0) {
                        printf("小队长:今天我来教大家如何把外界的魔法能量归引入体内，加深自身修为。\n");
                        if (neigong_level < 10) {
                            neigong_level++;
                            gengu++;
                            printf("你认真地听着，随后照着练习，大有收获。\n");
                            printf("基本内功等级+1，当前等级为%d；根骨+1，当前根骨为%d。\n", neigong_level, gengu);
                        }
                        else {
                            printf("这门基础魔法你已经学的差不多了，无法通过向别人学习继续提升了。\n");
                        }
                        Time(5, 0);
                    }
                    if (dialogue == 1) {
                        printf("小队长:今天我来教大家如何运用体内的魔法能量，以及如何调动外界的能量。\n");
                        if (waigong_level < 10) {
                            waigong_level++;
                            printf("你认真地听着，随后照着练习，大有收获。\n");
                            printf("基本外功等级+1，当前等级为%d。\n", waigong_level);
                        }
                        else {
                            printf("这门基础魔法你已经学的差不多了，无法通过向别人学习继续提升了。\n");
                        }
                        Time(5, 0);
                    }
                    if (dialogue == 2) {
                        printf("小队长:今天我来教大家如何利用魔法能量来实现自身的快速反应和快速移动。\n");
                        if (qinggong_level < 10) {
                            qinggong_level++;
                            shenfa++;
                            printf("你认真地听着，随后照着练习，大有收获。\n");
                            printf("基本轻功等级+1，当前等级为%d；身法+1，当前身法为%d。\n", qinggong_level, shenfa);
                        }
                        else{
                            printf("你已经学的差不多了，无法通过向别人学习继续提升了。\n");
                        }
                        Time(5, 0);
                    }
                }
                else if(hour <= 12){
                    printf("现在还没到小队长讲解基础魔法的时间。\n");
                    Time(0, 5);
                }
                else {
                    printf("现在不是小队长讲解基础魔法的时间，如果错过了只能明天再来了。\n");
                    Time(0, 5);
                }
            }
            else if (choice == 4)
            {
                printf("你向比武擂台出发了！\n");
                classroom1location = 1;
            }
            else if (choice == 5)
            { printf("你回到了寝室！\n");
                while(choice == 5){
                    printf("你现在在寝室。\n");
                    printf("你要休息多久？\n");
                    printf("请输入1到10之间的整数，单位:小时，输入0离开寝室。\n");
                    scanf("%d", &choice1);
                    if(choice1 > 0&&choice1 < 11){
                        player.HP+= 10*choice1;
                        if (player.HP > player.HPMAX) {
                            player.HP = player.HPMAX;
                        }
                        printf("你休息了%d个小时，感觉精神多了。\n", choice1);
                        printf("你恢复了%d点健康值，当前健康值为%d。\n", 10*choice1, player.HP);
                        Time(choice1, 0);
                    }
                    else if (choice1 == 0) {
                        printf("你离开了寝室。\n");
                        choice = 0; 
                        continue;
                    }
                    else {
                        printf("你是不是吃饱了撑着？\n"); 
                    }
                }
            }
            else if (choice == 6)
            {
                printf("你决定去砍树劈柴，锻炼一下根骨。\n");
                sleep(1);
                if (hour <= 5) {
                    player.HP-= 10*(6-hour);
                    printf("你正在熬夜，熬夜时进行任何活动都会扣除健康值。\n");
                    printf("你当前的健康值为%d点。\n", player.HP);
                }
                printf("忙了大半天，你只觉得腰酸背疼、四肢发软。\n");
                gengu++;
                gengu++;
                contribution++;
                printf("你的根骨提升了2点！你目前的根骨是%d。\n", gengu);
                printf("你获得了一点勇者协会贡献点！你目前拥有%d点贡献点。\n", contribution);
                Time(12, 0);
            }
            else if (choice == 7)
            {
                printf("你决定去空地，锻炼一下身法。\n");
                sleep(0.5);
                if (hour <= 5) {
                    player.HP-= 10*(6-hour);
                    printf("你正在熬夜，熬夜时进行任何活动都会扣除健康值。\n");
                    printf("你当前的健康值为%d点。\n", player.HP);
                    printf("只见空地上一只小猫正在窜来窜去，一会跑到天上，一会跑到地下，你看的眼都花了。\n");
                }
                else {
                    printf("只见空地上一只小猫正在窜来窜去，一会跑到天上，一会跑到地下，你看的眼都花了。\n");
                    printf("不过你看着几个满头大汗正追着那只猫的人，也就明白了那只猫是用来锻炼身法的道具。\n");
                }
                shenfa++;
                shenfa++;
                sleep(1);
                printf("你拼命地追呀追呀，但无论如何都抓不住那只猫，最终你累趴下了。\n");
                printf("你的身法提升了2点！你目前的身法是%d。\n", shenfa);
                Time(12, 0);
            }
            else if (choice == 8)
            {
                printf("你决定去挑水，锻炼一下臂力。\n");
                sleep(1);
                if (hour <= 5) {
                    player.HP-= 10*(6-hour);
                    printf("你正在熬夜，熬夜时进行任何活动都会扣除健康值。\n");
                    printf("你当前的健康值为%d点。\n", player.HP);
                }
                printf("忙了大半天，你只觉得腰酸背疼、四肢发软。\n");
                bili++;
                bili++;
                contribution++;
                printf("你的臂力提升了2点！你目前的臂力是%d。\n", bili);
                printf("你获得了一点勇者协会贡献点！你目前拥有%d点贡献点。\n", contribution);
                Time(12, 0);
            }
            else if (choice == 2)
            {
                if (neigong_level >= 10) {
                    choice1 = 2;
                }
                else {
                    printf("基本内功达到10级后才能归引外界能量到体内！\n");
                    choice1 = 0;
                }
                
                while(choice1 == 2){
                    printf("你盘膝坐下，准备吸收外界的魔法能量。\n");
                    printf("小队长告诉过你，如果不间断地修炼，从F到F+大概要7天，从F+到F++需要的时间要翻倍。\n");
                    printf("你要修炼多久？\n");
                    printf("请输入1到24之间的整数，单位:小时，输入0站起离开。\n");
                    scanf("%d", &choice2);
                    if(choice2 > 0&&choice2 < 25){
                        player.HP+= 10*choice2;
                        if (player.HP > player.HPMAX) {
                            player.HP = player.HPMAX;
                        }
                        flag = 0;
                        energy+= choice2;
                        if (jingjie == 1&&energy >= 168) {
                            energy-= 168;
                            printf("你只觉得体内的能量突然不由自主的加快了流动，冲开了你浑身的经脉。\n");
                            printf("在周围人群的惊呼中，你爆发出了一股更恐怖的气息，你觉得你的力量更强大了。\n");
                            jingjie = 2;
                            printf("你的修为境界提升到了F+。\n");
                            attack0 = 40;
                            attack = attack0+weapon;
                            player.attack = attack;
                            player.HPMAX*= 2;
                            printf("你当前生命上限为为%d，攻击力为%d\n", player.HPMAX, player.attack);
                            flag = 1;
                        }
                        if (jingjie == 2&&energy >= 336) {
                            energy-= 336;
                            printf("你只觉得体内的能量突然不由自主的加快了流动，冲开了你浑身的经脉。\n");
                            printf("在周围人群的惊呼中，你爆发出了一股更恐怖的气息，你觉得你的力量更强大了。\n");
                            jingjie = 3;
                            printf("你的修为境界提升到了F++。\n");
                            attack0 = 80;
                            attack = attack0+weapon;
                            player.attack = attack;
                            player.HPMAX*= 2;
                            printf("你当前生命上限为为%d，攻击力为%d\n", player.HPMAX, player.attack);
                            flag = 1;
                        }
                        if (flag == 0) {
                            printf("你修炼了%d个小时，感觉体内的能量更雄厚了。\n", choice2);
                        }
                        
                        printf("你恢复了%d点健康值，当前健康值为%d。\n", 10*choice2, player.HP);
                        Time(choice2, 0);
                    }
                    else if (choice2 == 0) {
                        printf("你选择站起离开。\n");
                        choice1 = 0; 
                        continue;
                    }
                    else {
                        printf("你现在修为不够，只修炼不吃饭会饿死。\n"); 
                    }
                }
            }
            else
            {
                printf("输入有误，请重新输入！\n");
            }
        }
        else if (classroom1location == 1) // 比武擂台
        {
            if (hour <= 5) {
                player.HP-= 10;
                printf("你正在熬夜，熬夜时进行任何活动都会扣除健康值。\n");
                printf("你当前的健康值为%d点。\n", player.HP);
            }
            printf("你现在在比武擂台，你可以：\n");
            printf("1. 去对战木头人，练习魔法\n");
            printf("2. 去观战别人在实战擂台上的战斗\n");
            printf("3. 到实战擂台上和别人对战\n");
            printf("4. 到实战擂台下面观看历史战斗回放\n");
            printf("5. 向教室中央返回\n");
            printf("请输入你的选择：");
            scanf("%d", &choice);
            if (choice == 1)
            {
                printf("你选择去对战木头人。\n");
                printf("请选择你要对战的木头人的等级:\n");
                printf("1. 入门。\n");
                printf("2. 初级。\n");
                printf("3. 中级。\n");
                printf("请输入你的选择：");
                scanf("%d", &choice1);
              
                
            }
            else if (choice == 2)
            {
                printf("你选择去观战他人。\n");
                
                dialogue = rand() % 26;
                a = rand() % 40;
                b = rand() % 40;
                while(a==b){
                b = rand() % 40;
                }
                fight(&npc[a],&npc[b]);
                printf("你作为旁观者观看他人的战斗，收获了不少经验。");
                flag = 0;
                choice2 = rand() % 3+1;
                if (choice2 == 1&&waigong_level < 10&&waigong_level > 0) {
                    waigongexperience+= (enemydamage+playerdamage)*turn/20;
                    printf("你的外功经验增加了%d!", (enemydamage+playerdamage)*turn/20);
                    flag = 1;
                }
                if (choice2 == 2&&neigong_level < 10&&neigong_level > 0) {
                    neigongexperience+= (enemydamage+playerdamage)*turn/20;
                    printf("你的内功经验增加了%d!", (enemydamage+playerdamage)*turn/20);
                    flag = 1;
                }
                if (choice2 == 3&&qinggong_level < 10&&qinggong_level > 0) {
                    qinggongexperience+= (enemydamage+playerdamage)*turn/20;
                    printf("你的轻功经验增加了%d!", (enemydamage+playerdamage)*turn/20);
                    flag = 1;
                }
                if (flag == 1) {
                    printf("这些经验需要你再通过自己的实战转化才能提高基础魔法的等级。\n");
                    flag = 0;
                }
                if (waigong_level >= 10) {
                    printf("他们的基本外功并不比你高明，你从他们那里学不到什么经验。\n");
                }
                if (neigong_level >= 10) {
                    printf("他们的基本内功并不比你高明，你从他们那里学不到什么经验。\n");
                }
                if (qinggong_level >= 10) {
                    printf("他们的基本轻功并不比你高明，你从他们那里学不到什么经验。\n");
                }
                
                Time(0, turn*2);
                turn = 0;
            }
            else if (choice == 3)
            {
                printf("你选择去与真人对战。\n");
                a = rand() % 40;
                fight(&player,&npc[a]);
                
            }
            else if (choice == 4)
            {
                printf("你选择去观看擂台上的历史战斗回放。\n");
                printf("请选择你要观看的实战回放:\n");
                printf("1. 经典战斗回放\n");
                printf("2. 普通战斗回放\n");
                printf("3. 你的战斗回放\n");
                printf("请输入你的选择：");
                scanf("%d", &choice1);
                if (choice1 == 1) {
                    
                    if (switches[2] == 1) {
                 a = rand() % 8+32;
                b = rand() % 8+32;
                while(a==b){
                b = rand() % 8+32;
                }
                npc[a].HP=HPMAX;
                fight(&npc[a],&npc[b]);
                        liangong();
                        Time(0, turn*3);
                        turn = 0;
                        switches[2] = 0;
                    }
                    else {
                        printf("每天只能观看一次经典战斗回放！");
                    }
                }
                if (choice1 == 2) {
                    dialogue = rand() % 26;
                  a = rand() % 40;
                b = rand() % 40;
                while(a==b){
                b = rand() % 40;
                }
                fight(&npc[a],&npc[b]);
                  
                    printf("你作为旁观者观看他人的战斗，收获了不少经验。");
                    flag = 0;
                    choice2 = rand() % 3+1;
                    if (choice2 == 1&&waigong_level < 10&&waigong_level > 0) {
                        waigongexperience+= (enemydamage+playerdamage)*turn/20;
                        printf("你的外功经验增加了%d!", (enemydamage+playerdamage)*turn/20);
                        flag = 1;
                    }
                    if (choice2 == 2&&neigong_level < 10&&neigong_level > 0) {
                        neigongexperience+= (enemydamage+playerdamage)*turn/20;
                        printf("你的内功经验增加了%d!", (enemydamage+playerdamage)*turn/20);
                        flag = 1;
                    }
                    if (choice2 == 3&&qinggong_level < 10&&qinggong_level > 0) {
                        qinggongexperience+= (enemydamage+playerdamage)*turn/20;
                        printf("你的轻功经验增加了%d!", (enemydamage+playerdamage)*turn/20);
                        flag = 1;
                    }
                    if (flag == 1) {
                        printf("这些经验需要你再通过自己的实战转化才能提高基础魔法的等级。\n");
                        flag = 0;
                    }
                    if (waigong_level >= 10) {
                        printf("他们的基本外功并不比你高明，你从他们那里学不到什么经验。\n");
                    }
                    if (neigong_level >= 10) {
                        printf("他们的基本内功并不比你高明，你从他们那里学不到什么经验。\n");
                    }
                    if (qinggong_level >= 10) {
                        printf("他们的基本轻功并不比你高明，你从他们那里学不到什么经验。\n");
                    }
                    Time(0, turn);
                    turn = 0;
                }
                if (choice1 == 3) {
                    printf("你打算复盘一下自己的战斗。\n");
                    health = player.HP;
                    player.HP = player.HPMAX;
                    
                    player.HP = health;
                    liangong();
                    Time(0, turn*2);
                    turn = 0;
                }
            }
            else if (choice == 5)
            {
                printf("你向教室中央返回了！\n");
                classroom1location = 0;
            }
            else
            {
                printf("输入有误，请重新输入！\n");
            }
        }
        else if (classroom1location == 2) // 角落里的的小型图书馆
        {
            printf("你用只有自己知道的方法来到了这个角落里的小型图书馆。\n");
        }
    }
    return 0;
} 

    /*
    样例输出：
    王欣
    张菲
    刘婷
    周妮
    李芝
    陈琳
    赵琼
    孙晶
    黄萱
    吴泉
    */
