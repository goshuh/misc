hehe::C
=======

16-04-21
--------

X Macro
_______

举个例子：

    .. code:: cpp

       #define __DO(_) \
           __do##1(_##1); \
           __do##2(_##2); \
           __do##3(_##3)

       #define __do1(_) delete _
       #define __do2(_) std::cout << #_ << '\n'
       #define __do3(_) return _

       __DO(value);

       #undef __do1
       #undef __do2
       #undef __do3
       #undef __DO

实际上就成了

    .. code:: cpp

       delete value1;
       std::cout << "value2" << '\n'
       return value3;

这里用到了宏的 `Concatenation <https://gcc.gnu.org/onlinedocs/cpp/Concatenation.html>`_ 和 `Stringification <https://gcc.gnu.org/onlinedocs/cpp/Stringification.html>`_ 。

------

C++ 的 traits
_____________

这个东西是用来向模板里面引入 specialization 的，当然它自己实现的时候就是用 `Specialization <http://en.cppreference.com/w/cpp/language/template_specialization>`_  的。

举个例子：

    .. code:: cpp

       template<typename T>
       struct is_special {
           static constexpr bool value = false;
       };

       template<>
       struct is_special<int> {
           static constexpr bool value = true;
       };

       template<typename T>
       void do_extra(const T& t) { }

       template<>
       void do_extra<int>(const int& i) {
           do_extra_only_for_int(i);
       }

       template<typename T>
       void do_for_all(const T& t) {

           if(is_special<T>::value)
               t.special_method();

           do_the_same_for_all();

           do_extra(t);
       }

实际就是把 special 的东西抽出模板之外单独 specialize。

------

Shell 手动 getopt
_________________

举个例子（反正 :code:`zsh` 下好使）：

    .. code:: sh

       args=()

       while [ $# -ge 1 ]; do
           case $1 in
               --version) do_version        && shift 1 ;;
               --para1=*) do_para1 ${1##*=} && shift 1 ;;
               --para2=*) do_para2 ${2##*=} && shift 1 ;;
               *)         args+=$1          && shift 1 ;;
           esac
       done

       echo ${args[@]}

其中用到了 Shell 的字符串匹配。

`back <../portal.rst>`_
