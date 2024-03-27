# bind_forward_template例子
```c++
template<typename F, typename...Args>
auto template_bf_func(F&& f, Args&&...args)
{
    return std::bind(std::forward<F>(f), std::forward<Args>(args)...);
}

void template_bind_forward_func()
{
    using func_type = std::function<void()>;
    std::queue<func_type> func_que;
    int c =100;
    auto task = template_bf_func([&](int a, int b){
        std::cout<<a+b+c<<std::endl;
    }, 10, 20);
    func_que.push(task);
    auto tsk = func_que.front();
    tsk();
}
```


```c++
template<typename F, typename...Args>
auto template_bf_func(F&& f, Args&&...args)
{
    return std::bind(std::forward<F>(f), std::forward<Args>(args)...);
}

template<typename T>
void rlfunc(T&& t)
{
    t++;
}

void template_bind_forward_func()
{    
    using func_type = std::function<void()>;
    std::queue<func_type> func_que;
    int val_1 = 100;
    int& c =val_1;
    std::string str = "hello";
    auto task = template_bf_func([](int& a, std::string& b){
        std::cout<<a<<std::endl;
        std::cout<<b<<std::endl;
        a++;
    }, std::ref(c), str);

    func_que.push(task);
    auto tsk = func_que.front();
    tsk();
    std::cout<<"main:"<<c<<std::endl;

    int val = 10;
    rlfunc(val);
    std::cout<<"main val:"<<val<<std::endl;
}


```
`std::ref(c)`传递参数，`lambda`函数中修改变量`c`，`c`在作用域中的只改变，参数传递`c`时，为值传递。