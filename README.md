#include <iostream>

#include <algorithm>

#include <vector>

using namespace std;

// تعرسف المتغيرات

double waiting_avrage = 0, w = 0;

double completion_avrage = 0, c = 0;



void FCFS(int size, vector<int> &time_counter) // دالة للخوارزمية الذي ياتي اولا يخدم اولا

{

    // حساب المتوسط لكلا من الانتظار والاكتمال

    for (int i = 0; i < size; i++)

    {

        cout << time_counter[i] << " ";

        w += time_counter[i];

        waiting_avrage += w;

        c += time_counter[i + 1];

        completion_avrage += c;

    }

    cout << "\nThe AVG waiting time= " << waiting_avrage / size;       // متوسط الانتظار

    cout << "\nThe AVG completion time= " << completion_avrage / size; // متوسط الاكتمال

    waiting_avrage = 0;

    completion_avrage = 0;

    w = 0;

    c = 0;

}



void SJF(int size, vector<int> &time_counter) // دالة لخورزمية الاقصر يخدم اولا

{

    sort(time_counter.begin(), time_counter.end()); // نرتب القيم

    // عملية حساب الانتظار ولالاكتمال

    for (int i = 0; i < size; i++)

    {

        w += time_counter[i];

        waiting_avrage += w;

        {

            c += time_counter[i + 1];

            completion_avrage += c;

        }

    }

    cout << "\nThe AVG waiting time= " << waiting_avrage / size;

    cout << "\nThe AVG completion time= " << completion_avrage / size;

    waiting_avrage = 0;

    completion_avrage = 0;

    w = 0;

    c = 0;

}



void RR(int size, vector<int> &time_counter) // الجدولة الدائرية

{

    cout << "Enter the time quantum: ";

    int quantum; // محدد الوقت

    cin >> quantum;

    vector<int> remaining_time(size, 0);  // فيكتور لحساب الباقي

    vector<int> waiting_time(size, 0);    // قيكتور لحساب وقت الانتظار

    vector<int> completion_time(size, 0); // فيكتور لحساب وقت الاكتمال

    int time = 0;                         // خزان

    int completed = 0;                    // عداد

    // اسناد قيم الوقت لكل عملية الى فيكتور الباقي

    for (int i = 0; i < size; i++)

        remaining_time[i] = time_counter[i + 1];

    while (completed < size) // التحقق من ان العدد يكون اقل من عدد العمليات

    {

        for (int i = 0; i < size; ++i)

        {

            if (remaining_time[i] > 0) // التحقق من ان وفت المعلية اكبر من الصفر

            {

                if (remaining_time[i] > quantum) // التحقق من ان وقت العملية اكبر من الوقت المحدد لها

                {

                    time += quantum;

                    remaining_time[i] -= quantum;

                }

                else

                {

                    time += remaining_time[i];

                    completion_time[i] = time;

                    remaining_time[i] = 0;

                    completed++;

                }

            }

        }

    }



    for (int i = 0; i < size; ++i) // عملية حساب زمن الانتظار والاكتمال

    {

        waiting_time[i] = completion_time[i] - time_counter[i + 1];

        waiting_avrage += waiting_time[i];

        completion_avrage += completion_time[i];

    }



    cout << "\nThe AVG waiting time= " << waiting_avrage / size;       // متوسط زمن الانتظار

    cout << "\nThe AVG completion time= " << completion_avrage / size; // متوسط زمن الاكتمال

}



int main()

{

    cout << "\n\n====== Welcome to our program ==========\n\n";



    cout << "Enter how many number of processes: ";

    int number_process;

    cin >> number_process;

    vector<int> time_counter(number_process + 1);



    cout << "Now enter time_counter for each one: \n";

    for (int i = 0; i < number_process; i++)

    {

        cout << "Input the time_counter for this process: " << i + 1 << endl;

        cin >> time_counter[i + 1];

    }

    time_counter[0] = 0;

    // تعريف ثلاث دوال للجدولة

    cout << "NOW you have three algorithm choices, choose one of them: \n";

    cout << "     1- First_Come_Served_First (FCSF) algorithm\n";

    cout << "     2- Shortest_Job_First (SJF)\n";

    cout << "     3- Round Robin (RR)\n";

    int choice;

    cout << "Enter your choice: ";

    cin >> choice;



    if (choice == 1)

    {

        FCFS(number_process, time_counter);

    }

    else if (choice == 2)

    {

        SJF(number_process, time_counter);

    }

    else if (choice == 3)

    {

        RR(number_process, time_counter);

    }

    else

    {

        cout << "Invalid choice!" << endl;

    }

}

    
