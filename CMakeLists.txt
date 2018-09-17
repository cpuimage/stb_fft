cmake_minimum_required(VERSION 2.8)
project(stb_fft_demo)

add_executable(stb_fft_demo example.c)
target_link_libraries(stb_fft_demo -lm)