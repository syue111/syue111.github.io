---
author: dashashi
comments: true
date: 2011-06-08 19:59:16+00:00
layout: post
slug: '%e5%8d%95%e5%87%bd%e6%95%b0%e7%89%88%e7%9a%84md5%e5%92%8csha1'
title: 单函数版的MD5和SHA1
wordpress_id: 1230
categories:
- 写程序
tags:
- 3D
- MD5
- SHA1
- 单函数
---

需要用到MD5跟SHA1算法对字符串做一些处理，网上找来的都是很多函数组合起来的，用起来比较麻烦，干脆自己写了个单函数版本的，贴上来吧- -|

<!-- more -->

    
    //MD5.cpp
    #include <string>
    #include <iostream>
    
    using namespace std;
    
    typedef unsigned long DWORD;
    
    std::string MD5(std::string s)
    {
    	const char HEX_CHAR[]={'0', '1', '2', '3', '4', '5', '6', '7',
    		'8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};
    
    	const DWORD Md5Calc_t[ 64 ] = {
    		0xd76aa478, 0xe8c7b756, 0x242070db, 0xc1bdceee,
    		0xf57c0faf, 0x4787c62a, 0xa8304613, 0xfd469501,
    		0x698098d8, 0x8b44f7af, 0xffff5bb1, 0x895cd7be,
    		0x6b901122, 0xfd987193, 0xa679438e, 0x49b40821,
    
    		0xf61e2562, 0xc040b340, 0x265e5a51, 0xe9b6c7aa,
    		0xd62f105d, 0x02441453, 0xd8a1e681, 0xe7d3fbc8,
    		0x21e1cde6, 0xc33707d6, 0xf4d50d87, 0x455a14ed,
    		0xa9e3e905, 0xfcefa3f8, 0x676f02d9, 0x8d2a4c8a,
    
    		0xfffa3942, 0x8771f681, 0x6d9d6122, 0xfde5380c,
    		0xa4beea44, 0x4bdecfa9, 0xf6bb4b60, 0xbebfbc70,
    		0x289b7ec6, 0xeaa127fa, 0xd4ef3085, 0x04881d05,
    		0xd9d4d039, 0xe6db99e5, 0x1fa27cf8, 0xc4ac5665,
    
    		0xf4292244, 0x432aff97, 0xab9423a7, 0xfc93a039,
    		0x655b59c3, 0x8f0ccc92, 0xffeff47d, 0x85845dd1,
    		0x6fa87e4f, 0xfe2ce6e0, 0xa3014314, 0x4e0811a1,
    		0xf7537e82, 0xbd3af235, 0x2ad7d2bb, 0xeb86d391
    	};
    
    	const DWORD Md5Calc_s[ 16 ] = { 7,12,17,22,5, 9,14,20, 4,11,16,23,6,10,15,21 };
    
    	//扩展成K*512位
    	DWORD *data;
    	int l;
    	l = s.length()*8;
    	data = new DWORD[((l/512)+1)*512];
    	memset(data, 0, sizeof(data[0])*((l/512)+1)*512);
    	for(unsigned int i = 0; i < s.length(); ++i){
    		data[i / 4] |= s[i] << 8*((i % 4));
    	}
    	data[s.length() / 4] |= 0x80 << 8*((s.length()%4));
    	data[((l/512)+1)*512/32-2]=l;
    	l = (l/512)+1;
    	//开始计算
    	DWORD H[4], a, b, c, d;
    	H[0]=0x67452301, H[1]=0xefcdab89, H[2]=0x98badcfe, H[3]=0x10325476;
    
    	for(int i = 0; i<l; ++i){
    		DWORD M[16];
    		for(int t = 0; t<16; ++t)
    			M[t] = data[i*16+t];
    
    		a = H[0], b = H[1], c = H[2], d = H[3];
    
    		DWORD s, k, x, e;
    		s = k = 0;
    		/** Turn 1, F */
    		s = k = 0;
    		for( x = 0; x < 16; x ++ ){
    			e = ( b & c ) | ( ~b & d );
    			e = a + e + M[ k ] + Md5Calc_t[ x ];
    			e = ( e >> ( 32 - Md5Calc_s[ s ] )) | ( e << Md5Calc_s[ s ] );
    			a = b + e;
    			e = d; d = c; c = b; b = a; a = e;
    			s = ( s + 1 ) % 4; k ++;
    		}
    
    		/** Turn 2, G */
    		k = 1; s = 4;
    		for( x = 16; x < 32; x ++ ){
    			e = ( b & d ) | ( c & ~d );
    			e = a + e + M[ k ] + Md5Calc_t[ x ];
    			e = ( e >> ( 32 - Md5Calc_s[ s ] )) | ( e << Md5Calc_s[ s ] );
    			a = b + e;
    			e = d; d = c; c = b; b = a; a = e;
    			k = ( k + 5 ) % 16;
    			s = ( s - 3 ) % 4 + 4;
    		}
    
    		/** Turn 3, H */
    		k = 5; s = 8;
    		for( x = 32; x < 48; x ++ ){
    			e = b ^ c ^ d;
    			e = a + e + M[ k ] + Md5Calc_t[ x ];
    			e = ( e >> ( 32 - Md5Calc_s[ s ] )) | ( e << Md5Calc_s[ s ] );
    			a = b + e;
    			e = d; d = c; c = b; b = a; a = e;
    			k = ( k + 3 ) % 16;
    			s = ( s - 7 ) % 4 + 8;
    		}
    
    		/** Turn 4, I */
    		k = 0; s = 12;
    		for( x = 48; x < 64; x ++ ){
    			e = c ^ ( b | ~d );
    			e = a + e + M[ k ] + Md5Calc_t[ x ];
    			e = ( e >> ( 32 - Md5Calc_s[ s ] )) | ( e << Md5Calc_s[ s ] );
    			a = b + e;
    			e = d; d = c; c = b; b = a; a = e;
    			k = ( k + 7 ) % 16;
    			s = ( s - 11 ) % 4  + 12;
    		}
    
    		H[0] += a;
    		H[1] += b;
    		H[2] += c;
    		H[3] += d;
    	}
    	char buf[33];
    	for(int i = 0; i<32; ++i){
    		buf[i] = HEX_CHAR[(H[i / 8] >> (4*((i % 8))))&0xf];
    	}
    	for(int i = 0; i<32; i += 2){
    		char tmp = buf[i];
    		buf[i] = buf[i+1];
    		buf[i+1] = tmp;
    	}
    	buf[32] = '\0';
    	return buf;
    }
    
    int main(int argc, char* argv[])
    {
    	char s[100];
    	while(true){
    		scanf("%s", s);
    		cout << MD5(s) << endl;
    	}
    	return 0;
    }




    
    //SHA1.cpp
    #include <string>
    #include <iostream>
    
    using namespace std;
    
    typedef unsigned long DWORD;
    
    std::string MD5(std::string s)
    {
    	const char HEX_CHAR[]={'0', '1', '2', '3', '4', '5', '6', '7',
    		'8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};
    
    	const DWORD Md5Calc_t[ 64 ] = {
    		0xd76aa478, 0xe8c7b756, 0x242070db, 0xc1bdceee,
    		0xf57c0faf, 0x4787c62a, 0xa8304613, 0xfd469501,
    		0x698098d8, 0x8b44f7af, 0xffff5bb1, 0x895cd7be,
    		0x6b901122, 0xfd987193, 0xa679438e, 0x49b40821,
    
    		0xf61e2562, 0xc040b340, 0x265e5a51, 0xe9b6c7aa,
    		0xd62f105d, 0x02441453, 0xd8a1e681, 0xe7d3fbc8,
    		0x21e1cde6, 0xc33707d6, 0xf4d50d87, 0x455a14ed,
    		0xa9e3e905, 0xfcefa3f8, 0x676f02d9, 0x8d2a4c8a,
    
    		0xfffa3942, 0x8771f681, 0x6d9d6122, 0xfde5380c,
    		0xa4beea44, 0x4bdecfa9, 0xf6bb4b60, 0xbebfbc70,
    		0x289b7ec6, 0xeaa127fa, 0xd4ef3085, 0x04881d05,
    		0xd9d4d039, 0xe6db99e5, 0x1fa27cf8, 0xc4ac5665,
    
    		0xf4292244, 0x432aff97, 0xab9423a7, 0xfc93a039,
    		0x655b59c3, 0x8f0ccc92, 0xffeff47d, 0x85845dd1,
    		0x6fa87e4f, 0xfe2ce6e0, 0xa3014314, 0x4e0811a1,
    		0xf7537e82, 0xbd3af235, 0x2ad7d2bb, 0xeb86d391
    	};
    
    	const DWORD Md5Calc_s[ 16 ] = { 7,12,17,22,5, 9,14,20, 4,11,16,23,6,10,15,21 };
    
    	//扩展成K*512位
    	DWORD *data;
    	int l;
    	l = s.length()*8;
    	data = new DWORD[((l/512)+1)*512];
    	memset(data, 0, sizeof(data[0])*((l/512)+1)*512);
    	for(unsigned int i = 0; i < s.length(); ++i){
    		data[i / 4] |= s[i] << 8*((i % 4));
    	}
    	data[s.length() / 4] |= 0x80 << 8*((s.length()%4));
    	data[((l/512)+1)*512/32-2]=l;
    	l = (l/512)+1;
    	//开始计算
    	DWORD H[4], a, b, c, d;
    	H[0]=0x67452301, H[1]=0xefcdab89, H[2]=0x98badcfe, H[3]=0x10325476;
    
    	for(int i = 0; i<l; ++i){
    		DWORD M[16];
    		for(int t = 0; t<16; ++t)
    			M[t] = data[i*16+t];
    
    		a = H[0], b = H[1], c = H[2], d = H[3];
    
    		DWORD s, k, x, e;
    		s = k = 0;
    		/** Turn 1, F */
    		s = k = 0;
    		for( x = 0; x < 16; x ++ ){
    			e = ( b & c ) | ( ~b & d );
    			e = a + e + M[ k ] + Md5Calc_t[ x ];
    			e = ( e >> ( 32 - Md5Calc_s[ s ] )) | ( e << Md5Calc_s[ s ] );
    			a = b + e;
    			e = d; d = c; c = b; b = a; a = e;
    			s = ( s + 1 ) % 4; k ++;
    		}
    
    		/** Turn 2, G */
    		k = 1; s = 4;
    		for( x = 16; x < 32; x ++ ){
    			e = ( b & d ) | ( c & ~d );
    			e = a + e + M[ k ] + Md5Calc_t[ x ];
    			e = ( e >> ( 32 - Md5Calc_s[ s ] )) | ( e << Md5Calc_s[ s ] );
    			a = b + e;
    			e = d; d = c; c = b; b = a; a = e;
    			k = ( k + 5 ) % 16;
    			s = ( s - 3 ) % 4 + 4;
    		}
    
    		/** Turn 3, H */
    		k = 5; s = 8;
    		for( x = 32; x < 48; x ++ ){
    			e = b ^ c ^ d;
    			e = a + e + M[ k ] + Md5Calc_t[ x ];
    			e = ( e >> ( 32 - Md5Calc_s[ s ] )) | ( e << Md5Calc_s[ s ] );
    			a = b + e;
    			e = d; d = c; c = b; b = a; a = e;
    			k = ( k + 3 ) % 16;
    			s = ( s - 7 ) % 4 + 8;
    		}
    
    		/** Turn 4, I */
    		k = 0; s = 12;
    		for( x = 48; x < 64; x ++ ){
    			e = c ^ ( b | ~d );
    			e = a + e + M[ k ] + Md5Calc_t[ x ];
    			e = ( e >> ( 32 - Md5Calc_s[ s ] )) | ( e << Md5Calc_s[ s ] );
    			a = b + e;
    			e = d; d = c; c = b; b = a; a = e;
    			k = ( k + 7 ) % 16;
    			s = ( s - 11 ) % 4  + 12;
    		}
    
    		H[0] += a;
    		H[1] += b;
    		H[2] += c;
    		H[3] += d;
    	}
    	char buf[33];
    	for(int i = 0; i<32; ++i){
    		buf[i] = HEX_CHAR[(H[i / 8] >> (4*((i % 8))))&0xf];
    	}
    	for(int i = 0; i<32; i += 2){
    		char tmp = buf[i];
    		buf[i] = buf[i+1];
    		buf[i+1] = tmp;
    	}
    	buf[32] = '\0';
    	return buf;
    }
    
    std::string SHA1(std::string s)
    {
    	const char HEX_CHAR[]={'0', '1', '2', '3', '4', '5', '6', '7',
    							'8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};
    	const DWORD K[] = {0x5A827999, 0x6ED9EBA1, 0x8F1BBCDC, 0xCA62C1D6};
    	//扩展成K*512位
    	DWORD *data;
    	int l;
    	l = s.length()*8;
    	data = new DWORD[((l/512)+1)*512];
    	memset(data, 0, sizeof(data[0])*((l/512)+1)*512);
    	for(unsigned int i = 0; i < s.length(); ++i){
    		data[i / 4] |= s[i] << 8*(3 - (i % 4));
    	}
    	data[s.length() / 4] |= 0x80 << 8*(3-(s.length()%4));
    	data[((l/512)+1)*512/32-1]=l;
    	l = (l/512)+1;
    	//开始计算
    	DWORD H[5], G[5];
    	H[0] = G[0] = 0x67452301;
    	H[1] = G[1] = 0xEFCDAB89;
    	H[2] = G[2] = 0x98BADCFE;
    	H[3] = G[3] = 0x10325476;
    	H[4] = G[4] = 0xC3D2E1F0;
    	for(int i = 0; i<l; ++i){
    		DWORD W[80];
    		int t;
    		for(t = 0; t<16; ++t)
    			W[t] = data[i*16+t];
    		for(t = 16; t<80; ++t){
    			DWORD tmp = W[t-3] ^ W[t-8] ^ W[t-14] ^ W[t-16];
    			W[t] = (tmp << 1)|(tmp >> 31);
    		}
    		DWORD tmp;
    		for(t = 0; t<5; ++t)
    			H[t] = G[t];
    		for(t = 0; t<20; ++t){
    			tmp = ((H[0] << 5) | (H[0] >> 27)) + ((H[1] & H[2]) | (~ H[1] & H[3])) + H[4] + W[t] + K[0];
    			H[4] = H[3]; H[3] = H[2]; H[2] = (H[1]<<30)|(H[1] >> 2); H[1] = H[0]; H[0] = tmp;
    		}
    		for(t = 20; t<40; ++t){
    			tmp = ((H[0] << 5) | (H[0] >> 27)) + (H[1] ^ H[2] ^ H[3]) + H[4] + W[t] + K[1];
    			H[4] = H[3]; H[3] = H[2]; H[2] = (H[1]<<30)|(H[1] >> 2); H[1] = H[0]; H[0] = tmp;
    		}
    		for(t = 40; t<60; ++t){
    			tmp = ((H[0] << 5) | (H[0] >> 27)) + ((H[1] & H[2])|(H[2] & H[3])|(H[1] & H[3])) + H[4] + W[t] + K[2];
    			H[4] = H[3]; H[3] = H[2]; H[2] = (H[1]<<30)|(H[1] >> 2); H[1] = H[0]; H[0] = tmp;
    		}
    		for(t = 60; t<80; ++t){
    			tmp = ((H[0] << 5) | (H[0] >> 27)) + (H[1] ^ H[2] ^ H[3]) + H[4] + W[t] + K[3];
    			H[4] = H[3]; H[3] = H[2]; H[2] = (H[1]<<30)|(H[1] >> 2); H[1] = H[0]; H[0] = tmp;
    		}
    		for(t = 0; t<5; ++t)
    			G[t] += H[t];
    	}
    	delete data;
    	char buf[41];
    	for(int i = 0; i<40; ++i){
    		buf[i] = HEX_CHAR[(G[i / 8] >> (4*(7- (i % 8))))&0xf];
    	}
    	buf[40] = '\0';
    	return buf;
    }
    
    int main(int argc, char* argv[])
    {
    	char s[100];
    	while(true){
    		scanf("%s", s);
    		cout << MD5("cb3%$%^$7xce" + SHA1("b4@789#^fc5"+string(s)+"*^v%$2!#@a$*(m@"))<< endl;
    	}
    	return 0;
    }



