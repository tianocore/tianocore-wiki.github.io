# Third-Party Security Advisories

|Component|CVE|Publication Date|EDK Impact|EDK2 Assessment|
|---------|---|----------------|----------|---------------|
|OpenSSL|CVE-2026-35189|9/29/2026|No|Not a problem for EDK2. EDK2 TlsLib does not support certificate revocation lists for certificate validation.|
|OpenSSL|CVE-2026-35191|9/29/2026|No|Not a problem for EDK2. QUIC is disabled by default in the CryptoPkg configuration.|
|OpenSSL|CVE-2026-42772|9/29/2026|No|Not a problem for EDK2. QUIC is disabled by default in the CryptoPkg configuration.|
|OpenSSL|CVE-2026-54872|9/29/2026|No|Not a problem for EDK2. EDK2 does not perform a context switch.|
|OpenSSL|CVE-2026-54873|9/29/2026|No|Not a problem for EDK2. QUIC is disabled by default in the CryptoPkg configuration.|
|OpenSSL|CVE-2026-54875|9/29/2026|No|Not a problem for EDK2. SM2 is not used by EDK2 CryptoPkg.|
|OpenSSL|CVE-2026-72896|9/29/2026|No|Not a problem for EDK2. RFC3779 is disabled by default in the CryptoPkg configuration.|
|OpenSSL|CVE-2026-72897|9/29/2026|No|Not a problem for EDK2. EDK2 does not perform a context switch.|
|OpenSSL|CVE-2026-75804|9/29/2026|No|Not a problem for EDK2. QUIC is disabled by default in the CryptoPkg configuration.|
|OpenSSL|CVE-2026-75805|9/29/2026|No|Not a problem for EDK2. CMP is disabled by default in the CryptoPkg configuration.|
|OpenSSL|CVE-2026-75806|9/29/2026|No|Not a problem for EDK2. DTLS is disabled by default in the CryptoPkg configuration.|
|OpenSSL|CVE-2026-77696|9/29/2026|No|Not a problem for EDK2. SM2 is not used by EDK2 CryptoPkg.|
|OpenSSL|CVE-2026-84782|9/29/2026|No|Not a problem for EDK2. DTLS is disabled by default in the CryptoPkg configuration.|
|OpenSSL|CVE-2026-84783|9/29/2026|No|Not a problem for EDK2. EDK2 uses OpenSSL 3.X and does not support OpenSSL 4.0.|
|OpenSSL|CVE-2026-84784|9/29/2026|No|Not a problem for EDK2. QUIC is disabled by default in the CryptoPkg configuration.|
|OpenSSL|CVE-2026-14457|8/25/2026|No|Not a problem for EDK2. EDK2 does not enable RPK.|
|OpenSSL|CVE-2026-18798|8/25/2026|No|Not a problem for EDK2. EDK2 does not support QUIC.|
|OpenSSL|CVE-2026-54874|8/25/2026|No|Not a problem for EDK2. EDK2 does not support DTLS.|
|OpenSSL|CVE-2026-63072|8/25/2026|No|Not a problem for EDK2. EDK2 does not support CMS.|
|OpenSSL|CVE-2026-63073|8/25/2026|No|Not a problem for EDK2. EDK2 does not support CMP.|
|OpenSSL|CVE-2026-63074|8/25/2026|No|Not a problem for EDK2. EDK2 does not support CMP.|
|OpenSSL|CVE-2026-63075|8/25/2026|No|Not a problem for EDK2. EDK2 does not support QUIC.|
|OpenSSL|CVE-2026-63076|8/25/2026|No|Not a problem for EDK2. EDK2 does not support CMP.|
|OpenSSL|CVE-2026-75803|8/25/2026|No|Not a problem for EDK2. EDK2 does not use EVP_Cipher().|
|OpenSSL|CVE-2026-14456|8/13/2026|No|Not a problem for EDK2. EDK2 does not support QUIC.|
|OpenSSL|CVE-2026-54876|8/13/2026|No|Not a problem for EDK2. EDK2 does not support OCSP.|
|OpenSSL|CVE-2026-34180|6/9/2026|Yes|The code might pass a certificate or signature larger than 2 GB. Recommendation Take the CryptoPkg update to OpenSSL 3.5.7 (https://github.com/tianocore/edk2/pull/12669).|
|OpenSSL|CVE-2026-34181|6/9/2026|Yes|PKCS12 is used in TlsLib. Recommendation Take the CryptoPkg update to OpenSSL 3.5.7 (https://github.com/tianocore/edk2/pull/12669).|
|OpenSSL|CVE-2026-45447|6/9/2026|Yes|The affected code is used in Authenticode and PKCS7 Verification. Recommendation Take the CryptoPkg update to OpenSSL 3.5.7 (https://github.com/tianocore/edk2/pull/12669).|
|OpenSSL|CVE-2026-34182|6/9/2026|No|Not a problem for EDK2. EDK2 does not support CMS.|
|OpenSSL|CVE-2026-34183|6/9/2026|No|Not a problem for EDK2. EDK2 does not support quic.|
|OpenSSL|CVE-2026-35188|6/9/2026|No|Not a problem for EDK2. EDK2 does not support OCSP.|
|OpenSSL|CVE-2026-42764|6/9/2026|No|Not a problem for EDK2. EDK2 does not support quic.|
|OpenSSL|CVE-2026-42765|6/9/2026|No|Not a problem for EDK2. EDK2 does not support OCSP.|
|OpenSSL|CVE-2026-42766|6/9/2026|No|Not a problem for EDK2. EDK2 does not support CMS.|
|OpenSSL|CVE-2026-42767|6/9/2026|No|Not a problem for EDK2. EDK2 does not support CMP.|
|OpenSSL|CVE-2026-42768|6/9/2026|No|Not a problem for EDK2. EDK2 does not use the affected code.|
|OpenSSL|CVE-2026-42769|6/9/2026|No|Not a problem for EDK2. EDK2 does not support CMP.|
|OpenSSL|CVE-2026-42770|6/9/2026|No|Not a problem for EDK2. EDK2 does not support the im|pacted case.|
|OpenSSL|CVE-2026-42771|6/9/2026|No|Not a problem for EDK2. EDK2 uses OpenSSL 3.X and does not support OpenSSL 4.0.|
|OpenSSL|CVE-2026-45445|6/9/2026|No|Not a problem for EDK2. EDK2 does not use EVP_Cipher().|
|OpenSSL|CVE-2026-45446|6/9/2026|No|Not a problem for EDK2. EDK2 does not support the impacted case.|
|OpenSSL|CVE-2026-7383|6/9/2026|No|Not a problem for EDK2. EDK2 does not use ASN1_mbstring_copy.|
|OpenSSL|CVE-2026-9076|6/9/2026|No|Not a problem for EDK2. EDK2 does not support CMS.|
|OpenSSL|CVE-2026-28386|4/7/2026|No|Not a problem for EDK2. EDK2 does not enable AES-CFB.|
|OpenSSL|CVE-2026-28387|4/7/2026|No|Not a problem for EDK2. DK2 TLS code does not enable DANE.|
|OpenSSL|CVE-2026-28388|4/7/2026|No|Not a problem for EDK2. EDK2 does not enable X509_V_FLAG_USE_DELTAS.|
|OpenSSL|CVE-2026-28389|4/7/2026|No|Not a problem for EDK2. EDK2 does not use PKCS7/CMS_decrypt.|
|OpenSSL|CVE-2026-28390|4/7/2026|No|Not a problem for EDK2. EDK2 does not use PKCS7/CMS_decrypt.|
|OpenSSL|CVE-2026-31789|4/7/2026|No|Not a problem for EDK2. EDK2 does not use OpenSSL print/log.|
|OpenSSL|CVE-2026-31790|4/7/2026|No|Not a problem for EDK2. EDK2 does not use EVP_PKEY_encapsulate.|
|OpenSSL|CVE-2026-2673|3/13/2026|No|Not a problem for EDK2. EDK2 does not support TLS1.3.|
|OpenSSL|CVE-2025-11187|1/27/2026|No|Not a problem for EDK2. EDK2 CryptoPkg does not support PKCS#12.|
|OpenSSL|CVE-2025-15467|1/27/2026|No|Not a problem for EDK2. EDK2 CryptoPkg does not support CMS.|
|OpenSSL|CVE-2025-15468|1/27/2026|No|Not a problem for EDK2. EDK2 CryptoPkg does not use SSL_CIPHER_find().|
|OpenSSL|CVE-2025-15469|1/27/2026|No|Not a problem for EDK2. EDK2 CryptoPkg does not use OpenSSL applications| including the ""openssl dgst"" command-line tool.|
|OpenSSL|CVE-2025-66199|1/27/2026|No|Not a problem for EDK2. EDK2 CryptoPkg does not support CompressedCertificate. All compression algorithms (brotli| zlib| zstd) are disabled by default.|
|OpenSSL|CVE-2025-68160|1/27/2026|No|Not a problem for EDK2. EDK2 CryptoPkg does not have a use case for BIO_f_linebuffer.|
|OpenSSL|CVE-2025-69418|1/27/2026|No|Not a problem for EDK2. EDK2 CryptoPkg has OCB disabled by default.|
|OpenSSL|CVE-2025-69419|1/27/2026|No|Not a problem for EDK2. EDK2 CryptoPkg does not support PKCS#12.|
|OpenSSL|CVE-2025-69420|1/27/2026|No|Not a problem for EDK2. EDK2 CryptoPkg has TS disabled by default.|
|OpenSSL|CVE-2025-69421|1/27/2026|No|Not a problem for EDK2. EDK2 CryptoPkg does not support PKCS#12.|
|OpenSSL|CVE-2025-9232|9/30/2025|No|Not a problem for EDK2. The OpenSSL HTTP implementation is not used by EDK2. EDK2 has its own HTTP implementation in NetworkPkg.|
|OpenSSL|CVE-2025-9231|9/30/2025|No|Not a problem for EDK2. SM2 is not used by EDK2 CryptoPkg.|
|OpenSSL|CVE-2025-9230|9/30/2025|No|Not a problem for EDK2. CMS is not used by EDK2 CryptoPkg.|
|OpenSSL|CVE-2024-12797|2/11/2025|No|Not a problem for EDK2. The OpenSSL API to enable Raw Public Keys (RPKs) is not available to users of EDK2 CryptoPkg.|
|OpenSSL|CVE-2024-9143|10/16/2024|No|Not a problem for EDK2. GF(2^M) Curves (EC2M) are not enabled in EDK2 CryptoPkg.|
|OpenSSL|CVE-2024-6119|2/11/2025|Yes|Impacts EDK2 TlsLib. The issue is only applicable to OpenSSL 3.* branches. Recommend moving to OpenSSL 3.0.15. The issue does not impact OpenSSL 1.1.1 branch.|
|OpenSSL|CVE-2024-5535|6/27/2024|No|Not a problem for EDK2. EDK2 does not call the SSL_select_next_proto function.|
|OpenSSL|CVE-2024-4741|5/28/2024|No|Not a problem for EDK2. EDK2 does not directly call the SSL_free_buffers function.|
|OpenSSL|CVE-2024-4603|5/16/2024|No|Not a problem for EDK2. EDK2 does not support DSA.|
|OpenSSL|CVE-2024-2511|4/08/2024|No|Not a problem for EDK2. This CVE only affects TLS servers supporting TLSv1.3. EDK2 does not support TLS server configurations. EDK2 does not support TLSv1.3.|
|OpenSSL|CVE-2024-0727|1/25/2024|No|Not a problem for EDK2. EDK2 CryptoPkg does not use the PKCS12 implementation or the SMIME_write_PKCS7() function.|
|OpenSSL|CVE-2023-6237|1/15/2024|No|No impact. The EDK2 CryptoPkg does not use the OpenSSL function EVP_PKEY_public_check().|
|OpenSSL|CVE-2023-6129|1/09/2024|No|No impact. The OpenSSL POLY1305 implementation is disabled by default in the EDK2 CryptoPkg configuration.|
|OpenSSL|CVE-2023-5678|11/06/2023|No|No impact. The affected DH params are not used by EDK2. EDK2 does not call any DH_check* related functions.|
|OpenSSL|CVE-2023-5363|10/25/2023|No|No impact. The affected functions that change the key and IV lengths are not called by EDK2 CryptoPkg.|
|OpenSSL|CVE-2023-4807|9/08/2023|No|No impact. The OpenSSL POLY1305 implementation is disabled by default in the CryptoPkg configuration.|
|OpenSSL|CVE-2023-3817|7/31/2023|No|No impact. DH code is used in CryptDh.c and TlsLib. In CryptoDh.c, EDK2 does not use DH_check() related functions and existing functions will not call into DH_check(). The TlsLib implementation is not affected by this issue.|
|OpenSSL|CVE-2023-3446|7/19/2023|No|No impact. The DH code of EDK2 does not call the DH_check() and related functions, or have the risk of using “p” parameters that are too large.|
|OpenSSL|CVE-2023-2975|7/14/2023|No|No impact. The OpenSSL AES-SIV cipher implementation is disabled by default in the CryptoPkg configuration.|
|OpenSSL|CVE-2023-2650|5/30/2023|Yes|OpenSSL 1.1.1u, updated in the edk2-stable202308 stable tag.|
|OpenSSL|CVE-2023-0466|3/28/2023|No|No Impact. EDK2 does not use the OpenSSL function X509_VERIFY_PARAM_add0_policy().|
|OpenSSL|CVE-2023-0465|3/28/2023|No|No Impact. According to the CVE description, ‘policy’ processing is disabled by default. For PKCS7, the X509 default parameter is used (NULL is passed in CryptoPkg\Library\BaseCryptLib\Pk\CryptPkcs7VerifyCommon.c). In X509_STORE_CTX_init(), ‘default’ parameter is used (CryptoPkg\Library\OpensslLib\openssl\crypto\x509\x509_vfy.c). For X509 verify, no policy parameter is set (CryptoPkg\Library\BaseCryptLib\Pk\CryptX509.c).|
|OpenSSL|CVE-2023-0464|3/22/2023|No|No Impact. According to the CVE description, ‘policy’ processing is disabled by default. For PKCS7, the X509 default parameter is used (NULL is passed in CryptoPkg\Library\BaseCryptLib\Pk\CryptPkcs7VerifyCommon.c). In X509_STORE_CTX_init(), ‘default’ parameter is used (CryptoPkg\Library\OpensslLib\openssl\crypto\x509\x509_vfy.c). For X509 verify, no policy parameter is set (CryptoPkg\Library\BaseCryptLib\Pk\CryptX509.c).|
|OpenSSL|CVE-2023-0286|2/08/2023|Yes|OpenSSL 1.1.1t, updated in the edk2-stable202305 stable tag.|
|OpenSSL|CVE-2023-0215|2/08/2023|Yes|OpenSSL 1.1.1t, updated in the edk2-stable202305 stable tag.|
|OpenSSL|CVE-2022-4450|2/08/2023|Yes|OpenSSL 1.1.1t, updated in the edk2-stable202305 stable tag.|
|OpenSSL|CVE-2022-4304|2/08/2023|Yes|OpenSSL 1.1.1t, updated in the edk2-stable202305 stable tag.|
|OpenSSL|CVE-2022-2097|7/05/2022|No|EDK2 does not enable AES OCB mode or use the 32-bit AES-NI assembly optimizations.|
|OpenSSL|CVE-2022-2068|6/21/2022|No|EDK2 never calls the c_rehash.in script in normal build process.|
|OpenSSL|CVE-2022-1292|5/03/2022|No|EDK2 never calls the c_rehash.in script in normal build process.|
|OpenSSL|CVE-2022-0778|3/15/2022|No|EDK2 does not use the BN_mod_sqrt() function.|
|OpenSSL|CVE-2021-4160|1/28/2022|No|The issue only affects MIPS platforms, and we don’t have MIPS in EDK2.|
|OpenSSL|CVE-2021-3712|8/24/2021|No|Analysis has confirmed that the relevant OpenSSL string operations have been used correctly and safely in EDK2.|
|OpenSSL|CVE-2021-3711|8/24/2021|No|The SM2 algorithm is not supported by EDK2.|
|OpenSSL|CVE-2021-3450|3/25/2021|No|X509_V_FLAG_X509_STRICT is never set by edk2.|
|OpenSSL|CVE-2021-3449|3/25/2021|No|Edk2 TLS supports client mode only. This issue only exists in server mode.|
