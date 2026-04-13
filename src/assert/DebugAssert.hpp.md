---
data:
  _extendedDependsOn: []
  _extendedRequiredBy:
  - icon: ':warning:'
    path: src/graph/GraphTemplate.hpp
    title: src/graph/GraphTemplate.hpp
  _extendedVerifiedWith: []
  _isVerificationFailed: false
  _pathExtension: hpp
  _verificationStatusIcon: ':warning:'
  attributes:
    links: []
  bundledCode: "#line 1 \"src/assert/DebugAssert.hpp\"\nnamespace debug_assert {\n\
    \ntemplate<class T>\nstd::string to_string_any(const T& value) {\n\tstd::ostringstream\
    \ oss;\n\toss << value;\n\treturn oss.str();\n}\n\ninline void collect_args(std::vector<std::string>&)\
    \ {}\n\ntemplate<class T, class... Rest>\nvoid collect_args(std::vector<std::string>&\
    \ out, T&& head, Rest&&... rest) {\n\tout.push_back(to_string_any(std::forward<T>(head)));\n\
    \tcollect_args(out, std::forward<Rest>(rest)...);\n}\n\ntemplate<class... Args>\n\
    std::string format(std::string_view fmt, Args&&... args) {\n\tstd::vector<std::string>\
    \ vals;\n\tvals.reserve(sizeof...(Args));\n\tcollect_args(vals, std::forward<Args>(args)...);\n\
    \n\tstd::string out;\n\tout.reserve(fmt.size() + vals.size() * 8);\n\n\tsize_t\
    \ arg_idx = 0;\n\tfor (size_t i = 0; i < fmt.size(); ++i) {\n\t\tif (fmt[i] ==\
    \ '{' && i + 1 < fmt.size() && fmt[i + 1] == '{') {\n\t\t\tout.push_back('{');\n\
    \t\t\t++i;\n\t\t\tcontinue;\n\t\t}\n\t\tif (fmt[i] == '}' && i + 1 < fmt.size()\
    \ && fmt[i + 1] == '}') {\n\t\t\tout.push_back('}');\n\t\t\t++i;\n\t\t\tcontinue;\n\
    \t\t}\n\t\tif (fmt[i] == '{' && i + 1 < fmt.size() && fmt[i + 1] == '}') {\n\t\
    \t\tif (arg_idx >= vals.size()) {\n\t\t\t\tthrow std::invalid_argument(\"format:\
    \ missing argument for '{}' placeholder\");\n\t\t\t}\n\t\t\tout += vals[arg_idx++];\n\
    \t\t\t++i;\n\t\t\tcontinue;\n\t\t}\n\t\tout.push_back(fmt[i]);\n\t}\n\n\tif (arg_idx\
    \ != vals.size()) {\n\t\tthrow std::invalid_argument(\"format: too many arguments\
    \ for format string\");\n\t}\n\n\treturn out;\n}\n\n[[noreturn]] inline void fail(const\
    \ char* expr, const char* file, int line, const char* func, const std::string&\
    \ msg) {\n\tstd::ostringstream oss;\n\toss << \"Assertion failed: (\" << expr\
    \ << \")\\n\"\n\t\t<< \"Location: \" << file << ':' << line << \" in \" << func;\n\
    \tif (!msg.empty()) {\n\t\toss << \"\\nMessage: \" << msg;\n\t}\n\tthrow std::runtime_error(oss.str());\n\
    }\n\n}  // namespace debug_assert\n\n#define assertf(cond, fmt, ...) \\\n\tdo\
    \ { \\\n\t\tif (!(cond)) { \\\n\t\t\t::debug_assert::fail(#cond, __FILE__, __LINE__,\
    \ __func__, ::debug_assert::format((fmt), ##__VA_ARGS__)); \\\n\t\t} \\\n\t} while\
    \ (0)\n\n#define panic(fmt, ...) \\\n\tdo { \\\n\t\t::debug_assert::fail(\"PANIC\"\
    , __FILE__, __LINE__, __func__, ::debug_assert::format((fmt), ##__VA_ARGS__));\
    \ \\\n\t} while (0)\n"
  code: "namespace debug_assert {\n\ntemplate<class T>\nstd::string to_string_any(const\
    \ T& value) {\n\tstd::ostringstream oss;\n\toss << value;\n\treturn oss.str();\n\
    }\n\ninline void collect_args(std::vector<std::string>&) {}\n\ntemplate<class\
    \ T, class... Rest>\nvoid collect_args(std::vector<std::string>& out, T&& head,\
    \ Rest&&... rest) {\n\tout.push_back(to_string_any(std::forward<T>(head)));\n\t\
    collect_args(out, std::forward<Rest>(rest)...);\n}\n\ntemplate<class... Args>\n\
    std::string format(std::string_view fmt, Args&&... args) {\n\tstd::vector<std::string>\
    \ vals;\n\tvals.reserve(sizeof...(Args));\n\tcollect_args(vals, std::forward<Args>(args)...);\n\
    \n\tstd::string out;\n\tout.reserve(fmt.size() + vals.size() * 8);\n\n\tsize_t\
    \ arg_idx = 0;\n\tfor (size_t i = 0; i < fmt.size(); ++i) {\n\t\tif (fmt[i] ==\
    \ '{' && i + 1 < fmt.size() && fmt[i + 1] == '{') {\n\t\t\tout.push_back('{');\n\
    \t\t\t++i;\n\t\t\tcontinue;\n\t\t}\n\t\tif (fmt[i] == '}' && i + 1 < fmt.size()\
    \ && fmt[i + 1] == '}') {\n\t\t\tout.push_back('}');\n\t\t\t++i;\n\t\t\tcontinue;\n\
    \t\t}\n\t\tif (fmt[i] == '{' && i + 1 < fmt.size() && fmt[i + 1] == '}') {\n\t\
    \t\tif (arg_idx >= vals.size()) {\n\t\t\t\tthrow std::invalid_argument(\"format:\
    \ missing argument for '{}' placeholder\");\n\t\t\t}\n\t\t\tout += vals[arg_idx++];\n\
    \t\t\t++i;\n\t\t\tcontinue;\n\t\t}\n\t\tout.push_back(fmt[i]);\n\t}\n\n\tif (arg_idx\
    \ != vals.size()) {\n\t\tthrow std::invalid_argument(\"format: too many arguments\
    \ for format string\");\n\t}\n\n\treturn out;\n}\n\n[[noreturn]] inline void fail(const\
    \ char* expr, const char* file, int line, const char* func, const std::string&\
    \ msg) {\n\tstd::ostringstream oss;\n\toss << \"Assertion failed: (\" << expr\
    \ << \")\\n\"\n\t\t<< \"Location: \" << file << ':' << line << \" in \" << func;\n\
    \tif (!msg.empty()) {\n\t\toss << \"\\nMessage: \" << msg;\n\t}\n\tthrow std::runtime_error(oss.str());\n\
    }\n\n}  // namespace debug_assert\n\n#define assertf(cond, fmt, ...) \\\n\tdo\
    \ { \\\n\t\tif (!(cond)) { \\\n\t\t\t::debug_assert::fail(#cond, __FILE__, __LINE__,\
    \ __func__, ::debug_assert::format((fmt), ##__VA_ARGS__)); \\\n\t\t} \\\n\t} while\
    \ (0)\n\n#define panic(fmt, ...) \\\n\tdo { \\\n\t\t::debug_assert::fail(\"PANIC\"\
    , __FILE__, __LINE__, __func__, ::debug_assert::format((fmt), ##__VA_ARGS__));\
    \ \\\n\t} while (0)\n"
  dependsOn: []
  isVerificationFile: false
  path: src/assert/DebugAssert.hpp
  requiredBy:
  - src/graph/GraphTemplate.hpp
  timestamp: '2026-04-13 17:46:56+09:00'
  verificationStatus: LIBRARY_NO_TESTS
  verifiedWith: []
documentation_of: src/assert/DebugAssert.hpp
layout: document
redirect_from:
- /library/src/assert/DebugAssert.hpp
- /library/src/assert/DebugAssert.hpp.html
title: src/assert/DebugAssert.hpp
---
