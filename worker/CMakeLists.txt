cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
cmake_policy(VERSION 3.0)
project(mediasoup-worker)
enable_language(ASM)

include(TestBigEndian)
test_big_endian(is_big_endian)

include(ExternalProject)

set(CMAKE_C_OUTPUT_EXTENSION_REPLACE 1)
set(CMAKE_CXX_OUTPUT_EXTENSION_REPLACE 1)

set(CMAKE_NINJA_FORCE_RESPONSE_FILE 1)

set(libmediasoup-worker_sources
  "src/lib.cpp"
  "src/DepLibSRTP.cpp"
  "src/DepLibUV.cpp"
  "src/DepLibWebRTC.cpp"
  "src/DepOpenSSL.cpp"
  "src/DepUsrSCTP.cpp"
  "src/Logger.cpp"
  "src/MediaSoupErrors.cpp"
  "src/Settings.cpp"
  "src/Worker.cpp"
  "src/Utils/Crypto.cpp"
  "src/Utils/File.cpp"
  "src/Utils/IP.cpp"
  "src/Utils/String.cpp"
  "src/handles/SignalsHandler.cpp"
  "src/handles/TcpConnectionHandler.cpp"
  "src/handles/TcpServerHandler.cpp"
  "src/handles/Timer.cpp"
  "src/handles/UdpSocketHandler.cpp"
  "src/handles/UnixStreamSocket.cpp"
  "src/Channel/ChannelNotifier.cpp"
  "src/Channel/ChannelRequest.cpp"
  "src/Channel/ChannelSocket.cpp"
  "src/PayloadChannel/Notification.cpp"
  "src/PayloadChannel/PayloadChannelNotifier.cpp"
  "src/PayloadChannel/PayloadChannelRequest.cpp"
  "src/PayloadChannel/PayloadChannelSocket.cpp"
  "src/RTC/AudioLevelObserver.cpp"
  "src/RTC/Consumer.cpp"
  "src/RTC/DataConsumer.cpp"
  "src/RTC/DataProducer.cpp"
  "src/RTC/DirectTransport.cpp"
  "src/RTC/DtlsTransport.cpp"
  "src/RTC/IceCandidate.cpp"
  "src/RTC/IceServer.cpp"
  "src/RTC/KeyFrameRequestManager.cpp"
  "src/RTC/NackGenerator.cpp"
  "src/RTC/PipeConsumer.cpp"
  "src/RTC/PipeTransport.cpp"
  "src/RTC/PlainTransport.cpp"
  "src/RTC/PortManager.cpp"
  "src/RTC/Producer.cpp"
  "src/RTC/RateCalculator.cpp"
  "src/RTC/Router.cpp"
  "src/RTC/RtpListener.cpp"
  "src/RTC/RtpObserver.cpp"
  "src/RTC/RtpPacket.cpp"
  "src/RTC/RtpProbationGenerator.cpp"
  "src/RTC/RtpStream.cpp"
  "src/RTC/RtpStreamRecv.cpp"
  "src/RTC/RtpStreamSend.cpp"
  "src/RTC/RtxStream.cpp"
  "src/RTC/SctpAssociation.cpp"
  "src/RTC/SctpListener.cpp"
  "src/RTC/SenderBandwidthEstimator.cpp"
  "src/RTC/SeqManager.cpp"
  "src/RTC/SimpleConsumer.cpp"
  "src/RTC/SimulcastConsumer.cpp"
  "src/RTC/SrtpSession.cpp"
  "src/RTC/StunPacket.cpp"
  "src/RTC/SvcConsumer.cpp"
  "src/RTC/TcpConnection.cpp"
  "src/RTC/TcpServer.cpp"
  "src/RTC/Transport.cpp"
  "src/RTC/TransportCongestionControlClient.cpp"
  "src/RTC/TransportCongestionControlServer.cpp"
  "src/RTC/TransportTuple.cpp"
  "src/RTC/TrendCalculator.cpp"
  "src/RTC/UdpSocket.cpp"
  "src/RTC/WebRtcTransport.cpp"
  "src/RTC/Codecs/H264.cpp"
  "src/RTC/Codecs/VP8.cpp"
  "src/RTC/Codecs/VP9.cpp"
  "src/RTC/RtpDictionaries/Media.cpp"
  "src/RTC/RtpDictionaries/Parameters.cpp"
  "src/RTC/RtpDictionaries/RtcpFeedback.cpp"
  "src/RTC/RtpDictionaries/RtcpParameters.cpp"
  "src/RTC/RtpDictionaries/RtpCodecMimeType.cpp"
  "src/RTC/RtpDictionaries/RtpCodecParameters.cpp"
  "src/RTC/RtpDictionaries/RtpEncodingParameters.cpp"
  "src/RTC/RtpDictionaries/RtpHeaderExtensionParameters.cpp"
  "src/RTC/RtpDictionaries/RtpHeaderExtensionUri.cpp"
  "src/RTC/RtpDictionaries/RtpParameters.cpp"
  "src/RTC/RtpDictionaries/RtpRtxParameters.cpp"
  "src/RTC/SctpDictionaries/SctpStreamParameters.cpp"
  "src/RTC/RTCP/Packet.cpp"
  "src/RTC/RTCP/CompoundPacket.cpp"
  "src/RTC/RTCP/SenderReport.cpp"
  "src/RTC/RTCP/ReceiverReport.cpp"
  "src/RTC/RTCP/Sdes.cpp"
  "src/RTC/RTCP/Bye.cpp"
  "src/RTC/RTCP/Feedback.cpp"
  "src/RTC/RTCP/FeedbackPs.cpp"
  "src/RTC/RTCP/FeedbackRtp.cpp"
  "src/RTC/RTCP/FeedbackRtpNack.cpp"
  "src/RTC/RTCP/FeedbackRtpTmmb.cpp"
  "src/RTC/RTCP/FeedbackRtpSrReq.cpp"
  "src/RTC/RTCP/FeedbackRtpTllei.cpp"
  "src/RTC/RTCP/FeedbackRtpEcn.cpp"
  "src/RTC/RTCP/FeedbackRtpTransport.cpp"
  "src/RTC/RTCP/FeedbackPsPli.cpp"
  "src/RTC/RTCP/FeedbackPsSli.cpp"
  "src/RTC/RTCP/FeedbackPsRpsi.cpp"
  "src/RTC/RTCP/FeedbackPsFir.cpp"
  "src/RTC/RTCP/FeedbackPsTst.cpp"
  "src/RTC/RTCP/FeedbackPsVbcm.cpp"
  "src/RTC/RTCP/FeedbackPsLei.cpp"
  "src/RTC/RTCP/FeedbackPsAfb.cpp"
  "src/RTC/RTCP/FeedbackPsRemb.cpp"
  "src/RTC/RTCP/XR.cpp"
  "src/RTC/RTCP/XrDelaySinceLastRr.cpp"
  "src/RTC/RTCP/XrReceiverReferenceTime.cpp"
)

set(libmediasoup-worker_headers
  "include/lib.hpp"
  "include/DepLibSRTP.hpp"
  "include/DepLibUV.hpp"
  "include/DepLibWebRTC.hpp"
  "include/DepOpenSSL.hpp"
  "include/DepUsrSCTP.hpp"
  "include/LogLevel.hpp"
  "include/Logger.hpp"
  "include/MediaSoupErrors.hpp"
  "include/Settings.hpp"
  "include/Utils.hpp"
  "include/Worker.hpp"
  "include/common.hpp"
  "include/handles/SignalsHandler.hpp"
  "include/handles/TcpConnectionHandler.hpp"
  "include/handles/TcpServerHandler.hpp"
  "include/handles/Timer.hpp"
  "include/handles/UdpSocketHandler.hpp"
  "include/handles/UnixStreamSocket.hpp"
  "include/Channel/ChannelNotifier.hpp"
  "include/Channel/ChannelRequest.hpp"
  "include/Channel/ChannelSocket.hpp"
  "include/PayloadChannel/Notification.hpp"
  "include/PayloadChannel/PayloadChannelNotifier.hpp"
  "include/PayloadChannel/PayloadChannelRequest.hpp"
  "include/PayloadChannel/PayloadChannelSocket.hpp"
  "include/RTC/BweType.hpp"
  "include/RTC/AudioLevelObserver.hpp"
  "include/RTC/Consumer.hpp"
  "include/RTC/DataConsumer.hpp"
  "include/RTC/DirectTransport.hpp"
  "include/RTC/DataProducer.hpp"
  "include/RTC/DtlsTransport.hpp"
  "include/RTC/IceCandidate.hpp"
  "include/RTC/IceServer.hpp"
  "include/RTC/KeyFrameRequestManager.hpp"
  "include/RTC/NackGenerator.hpp"
  "include/RTC/Parameters.hpp"
  "include/RTC/PipeConsumer.hpp"
  "include/RTC/PipeTransport.hpp"
  "include/RTC/PlainTransport.hpp"
  "include/RTC/PortManager.hpp"
  "include/RTC/Producer.hpp"
  "include/RTC/RateCalculator.hpp"
  "include/RTC/Router.hpp"
  "include/RTC/RtpDictionaries.hpp"
  "include/RTC/RtpHeaderExtensionIds.hpp"
  "include/RTC/RtpListener.hpp"
  "include/RTC/RtpObserver.hpp"
  "include/RTC/RtpPacket.hpp"
  "include/RTC/RtpProbationGenerator.hpp"
  "include/RTC/RtpStream.hpp"
  "include/RTC/RtpStreamRecv.hpp"
  "include/RTC/RtpStreamSend.hpp"
  "include/RTC/RtxStream.hpp"
  "include/RTC/SctpAssociation.hpp"
  "include/RTC/SctpDictionaries.hpp"
  "include/RTC/SctpListener.hpp"
  "include/RTC/SenderBandwidthEstimator.hpp"
  "include/RTC/SeqManager.hpp"
  "include/RTC/SimpleConsumer.hpp"
  "include/RTC/SimulcastConsumer.hpp"
  "include/RTC/SrtpSession.hpp"
  "include/RTC/StunPacket.hpp"
  "include/RTC/SvcConsumer.hpp"
  "include/RTC/TcpConnection.hpp"
  "include/RTC/TcpServer.hpp"
  "include/RTC/Transport.hpp"
  "include/RTC/TransportCongestionControlClient.hpp"
  "include/RTC/TransportCongestionControlServer.hpp"
  "include/RTC/TransportTuple.hpp"
  "include/RTC/TrendCalculator.hpp"
  "include/RTC/UdpSocket.hpp"
  "include/RTC/WebRtcTransport.hpp"
  "include/RTC/Codecs/Tools.hpp"
  "include/RTC/Codecs/PayloadDescriptorHandler.hpp"
  "include/RTC/Codecs/H264.hpp"
  "include/RTC/Codecs/VP8.hpp"
  "include/RTC/Codecs/VP9.hpp"
  "include/RTC/RTCP/Packet.hpp"
  "include/RTC/RTCP/CompoundPacket.hpp"
  "include/RTC/RTCP/SenderReport.hpp"
  "include/RTC/RTCP/ReceiverReport.hpp"
  "include/RTC/RTCP/Sdes.hpp"
  "include/RTC/RTCP/Bye.hpp"
  "include/RTC/RTCP/Feedback.hpp"
  "include/RTC/RTCP/FeedbackItem.hpp"
  "include/RTC/RTCP/FeedbackPs.hpp"
  "include/RTC/RTCP/FeedbackRtp.hpp"
  "include/RTC/RTCP/FeedbackRtpNack.hpp"
  "include/RTC/RTCP/FeedbackRtpTmmb.hpp"
  "include/RTC/RTCP/FeedbackRtpSrReq.hpp"
  "include/RTC/RTCP/FeedbackRtpTllei.hpp"
  "include/RTC/RTCP/FeedbackRtpEcn.hpp"
  "include/RTC/RTCP/FeedbackRtpTransport.hpp"
  "include/RTC/RTCP/FeedbackPsPli.hpp"
  "include/RTC/RTCP/FeedbackPsSli.hpp"
  "include/RTC/RTCP/FeedbackPsRpsi.hpp"
  "include/RTC/RTCP/FeedbackPsFir.hpp"
  "include/RTC/RTCP/FeedbackPsTst.hpp"
  "include/RTC/RTCP/FeedbackPsVbcm.hpp"
  "include/RTC/RTCP/FeedbackPsLei.hpp"
  "include/RTC/RTCP/FeedbackPsAfb.hpp"
  "include/RTC/RTCP/FeedbackPsRemb.hpp"
  "include/RTC/RTCP/XR.hpp"
  "include/RTC/RTCP/XrDelaySinceLastRr.hpp"
  "include/RTC/RTCP/XrReceiverReferenceTime.hpp"
)

# TODO: Below code is auto-generated for a single platform and fuzzer doesn't even compile

#mediasoup/worker/deps/openssl/openssl.gyp:openssl#target
list(APPEND openssl__asm_srcs
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/aes/aesni-mb-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/aes/aesni-sha1-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/aes/aesni-sha256-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/aes/aesni-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/aes/vpaes-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/bn/rsaz-avx2.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/bn/rsaz-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/bn/x86_64-gf2m.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/bn/x86_64-mont.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/bn/x86_64-mont5.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/camellia/cmll-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/chacha/chacha-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/ec/ecp_nistz256-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/ec/x25519-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/md5/md5-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/modes/aesni-gcm-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/modes/ghash-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/poly1305/poly1305-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/rc4/rc4-md5-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/rc4/rc4-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/sha/keccak1600-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/sha/sha1-mb-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/sha/sha1-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/sha/sha256-mb-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/sha/sha256-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/sha/sha512-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/whrlpool/wp-x86_64.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/x86_64cpuid.s"
  "deps/openssl/config/archs/linux-x86_64/asm_avx2/engines/e_padlock-x86_64.s")
list(APPEND openssl__c_srcs
  "deps/openssl/openssl/ssl/bio_ssl.c"
  "deps/openssl/openssl/ssl/d1_lib.c"
  "deps/openssl/openssl/ssl/d1_msg.c"
  "deps/openssl/openssl/ssl/d1_srtp.c"
  "deps/openssl/openssl/ssl/methods.c"
  "deps/openssl/openssl/ssl/packet.c"
  "deps/openssl/openssl/ssl/pqueue.c"
  "deps/openssl/openssl/ssl/record/dtls1_bitmap.c"
  "deps/openssl/openssl/ssl/record/rec_layer_d1.c"
  "deps/openssl/openssl/ssl/record/rec_layer_s3.c"
  "deps/openssl/openssl/ssl/record/ssl3_buffer.c"
  "deps/openssl/openssl/ssl/record/ssl3_record.c"
  "deps/openssl/openssl/ssl/record/ssl3_record_tls13.c"
  "deps/openssl/openssl/ssl/s3_cbc.c"
  "deps/openssl/openssl/ssl/s3_enc.c"
  "deps/openssl/openssl/ssl/s3_lib.c"
  "deps/openssl/openssl/ssl/s3_msg.c"
  "deps/openssl/openssl/ssl/ssl_asn1.c"
  "deps/openssl/openssl/ssl/ssl_cert.c"
  "deps/openssl/openssl/ssl/ssl_ciph.c"
  "deps/openssl/openssl/ssl/ssl_conf.c"
  "deps/openssl/openssl/ssl/ssl_err.c"
  "deps/openssl/openssl/ssl/ssl_init.c"
  "deps/openssl/openssl/ssl/ssl_lib.c"
  "deps/openssl/openssl/ssl/ssl_mcnf.c"
  "deps/openssl/openssl/ssl/ssl_quic.c"
  "deps/openssl/openssl/ssl/ssl_rsa.c"
  "deps/openssl/openssl/ssl/ssl_sess.c"
  "deps/openssl/openssl/ssl/ssl_stat.c"
  "deps/openssl/openssl/ssl/ssl_txt.c"
  "deps/openssl/openssl/ssl/ssl_utst.c"
  "deps/openssl/openssl/ssl/statem/extensions.c"
  "deps/openssl/openssl/ssl/statem/extensions_clnt.c"
  "deps/openssl/openssl/ssl/statem/extensions_cust.c"
  "deps/openssl/openssl/ssl/statem/extensions_srvr.c"
  "deps/openssl/openssl/ssl/statem/statem.c"
  "deps/openssl/openssl/ssl/statem/statem_clnt.c"
  "deps/openssl/openssl/ssl/statem/statem_dtls.c"
  "deps/openssl/openssl/ssl/statem/statem_lib.c"
  "deps/openssl/openssl/ssl/statem/statem_quic.c"
  "deps/openssl/openssl/ssl/statem/statem_srvr.c"
  "deps/openssl/openssl/ssl/t1_enc.c"
  "deps/openssl/openssl/ssl/t1_lib.c"
  "deps/openssl/openssl/ssl/t1_trce.c"
  "deps/openssl/openssl/ssl/tls13_enc.c"
  "deps/openssl/openssl/ssl/tls_srp.c"
  "deps/openssl/openssl/crypto/aes/aes_cbc.c"
  "deps/openssl/openssl/crypto/aes/aes_cfb.c"
  "deps/openssl/openssl/crypto/aes/aes_core.c"
  "deps/openssl/openssl/crypto/aes/aes_ecb.c"
  "deps/openssl/openssl/crypto/aes/aes_ige.c"
  "deps/openssl/openssl/crypto/aes/aes_misc.c"
  "deps/openssl/openssl/crypto/aes/aes_ofb.c"
  "deps/openssl/openssl/crypto/aes/aes_wrap.c"
  "deps/openssl/openssl/crypto/aria/aria.c"
  "deps/openssl/openssl/crypto/asn1/a_bitstr.c"
  "deps/openssl/openssl/crypto/asn1/a_d2i_fp.c"
  "deps/openssl/openssl/crypto/asn1/a_digest.c"
  "deps/openssl/openssl/crypto/asn1/a_dup.c"
  "deps/openssl/openssl/crypto/asn1/a_gentm.c"
  "deps/openssl/openssl/crypto/asn1/a_i2d_fp.c"
  "deps/openssl/openssl/crypto/asn1/a_int.c"
  "deps/openssl/openssl/crypto/asn1/a_mbstr.c"
  "deps/openssl/openssl/crypto/asn1/a_object.c"
  "deps/openssl/openssl/crypto/asn1/a_octet.c"
  "deps/openssl/openssl/crypto/asn1/a_print.c"
  "deps/openssl/openssl/crypto/asn1/a_sign.c"
  "deps/openssl/openssl/crypto/asn1/a_strex.c"
  "deps/openssl/openssl/crypto/asn1/a_strnid.c"
  "deps/openssl/openssl/crypto/asn1/a_time.c"
  "deps/openssl/openssl/crypto/asn1/a_type.c"
  "deps/openssl/openssl/crypto/asn1/a_utctm.c"
  "deps/openssl/openssl/crypto/asn1/a_utf8.c"
  "deps/openssl/openssl/crypto/asn1/a_verify.c"
  "deps/openssl/openssl/crypto/asn1/ameth_lib.c"
  "deps/openssl/openssl/crypto/asn1/asn1_err.c"
  "deps/openssl/openssl/crypto/asn1/asn1_gen.c"
  "deps/openssl/openssl/crypto/asn1/asn1_item_list.c"
  "deps/openssl/openssl/crypto/asn1/asn1_lib.c"
  "deps/openssl/openssl/crypto/asn1/asn1_par.c"
  "deps/openssl/openssl/crypto/asn1/asn_mime.c"
  "deps/openssl/openssl/crypto/asn1/asn_moid.c"
  "deps/openssl/openssl/crypto/asn1/asn_mstbl.c"
  "deps/openssl/openssl/crypto/asn1/asn_pack.c"
  "deps/openssl/openssl/crypto/asn1/bio_asn1.c"
  "deps/openssl/openssl/crypto/asn1/bio_ndef.c"
  "deps/openssl/openssl/crypto/asn1/d2i_pr.c"
  "deps/openssl/openssl/crypto/asn1/d2i_pu.c"
  "deps/openssl/openssl/crypto/asn1/evp_asn1.c"
  "deps/openssl/openssl/crypto/asn1/f_int.c"
  "deps/openssl/openssl/crypto/asn1/f_string.c"
  "deps/openssl/openssl/crypto/asn1/i2d_pr.c"
  "deps/openssl/openssl/crypto/asn1/i2d_pu.c"
  "deps/openssl/openssl/crypto/asn1/n_pkey.c"
  "deps/openssl/openssl/crypto/asn1/nsseq.c"
  "deps/openssl/openssl/crypto/asn1/p5_pbe.c"
  "deps/openssl/openssl/crypto/asn1/p5_pbev2.c"
  "deps/openssl/openssl/crypto/asn1/p5_scrypt.c"
  "deps/openssl/openssl/crypto/asn1/p8_pkey.c"
  "deps/openssl/openssl/crypto/asn1/t_bitst.c"
  "deps/openssl/openssl/crypto/asn1/t_pkey.c"
  "deps/openssl/openssl/crypto/asn1/t_spki.c"
  "deps/openssl/openssl/crypto/asn1/tasn_dec.c"
  "deps/openssl/openssl/crypto/asn1/tasn_enc.c"
  "deps/openssl/openssl/crypto/asn1/tasn_fre.c"
  "deps/openssl/openssl/crypto/asn1/tasn_new.c"
  "deps/openssl/openssl/crypto/asn1/tasn_prn.c"
  "deps/openssl/openssl/crypto/asn1/tasn_scn.c"
  "deps/openssl/openssl/crypto/asn1/tasn_typ.c"
  "deps/openssl/openssl/crypto/asn1/tasn_utl.c"
  "deps/openssl/openssl/crypto/asn1/x_algor.c"
  "deps/openssl/openssl/crypto/asn1/x_bignum.c"
  "deps/openssl/openssl/crypto/asn1/x_info.c"
  "deps/openssl/openssl/crypto/asn1/x_int64.c"
  "deps/openssl/openssl/crypto/asn1/x_long.c"
  "deps/openssl/openssl/crypto/asn1/x_pkey.c"
  "deps/openssl/openssl/crypto/asn1/x_sig.c"
  "deps/openssl/openssl/crypto/asn1/x_spki.c"
  "deps/openssl/openssl/crypto/asn1/x_val.c"
  "deps/openssl/openssl/crypto/async/arch/async_null.c"
  "deps/openssl/openssl/crypto/async/arch/async_posix.c"
  "deps/openssl/openssl/crypto/async/arch/async_win.c"
  "deps/openssl/openssl/crypto/async/async.c"
  "deps/openssl/openssl/crypto/async/async_err.c"
  "deps/openssl/openssl/crypto/async/async_wait.c"
  "deps/openssl/openssl/crypto/bf/bf_cfb64.c"
  "deps/openssl/openssl/crypto/bf/bf_ecb.c"
  "deps/openssl/openssl/crypto/bf/bf_enc.c"
  "deps/openssl/openssl/crypto/bf/bf_ofb64.c"
  "deps/openssl/openssl/crypto/bf/bf_skey.c"
  "deps/openssl/openssl/crypto/bio/b_addr.c"
  "deps/openssl/openssl/crypto/bio/b_dump.c"
  "deps/openssl/openssl/crypto/bio/b_print.c"
  "deps/openssl/openssl/crypto/bio/b_sock.c"
  "deps/openssl/openssl/crypto/bio/b_sock2.c"
  "deps/openssl/openssl/crypto/bio/bf_buff.c"
  "deps/openssl/openssl/crypto/bio/bf_lbuf.c"
  "deps/openssl/openssl/crypto/bio/bf_nbio.c"
  "deps/openssl/openssl/crypto/bio/bf_null.c"
  "deps/openssl/openssl/crypto/bio/bio_cb.c"
  "deps/openssl/openssl/crypto/bio/bio_err.c"
  "deps/openssl/openssl/crypto/bio/bio_lib.c"
  "deps/openssl/openssl/crypto/bio/bio_meth.c"
  "deps/openssl/openssl/crypto/bio/bss_acpt.c"
  "deps/openssl/openssl/crypto/bio/bss_bio.c"
  "deps/openssl/openssl/crypto/bio/bss_conn.c"
  "deps/openssl/openssl/crypto/bio/bss_dgram.c"
  "deps/openssl/openssl/crypto/bio/bss_fd.c"
  "deps/openssl/openssl/crypto/bio/bss_file.c"
  "deps/openssl/openssl/crypto/bio/bss_log.c"
  "deps/openssl/openssl/crypto/bio/bss_mem.c"
  "deps/openssl/openssl/crypto/bio/bss_null.c"
  "deps/openssl/openssl/crypto/bio/bss_sock.c"
  "deps/openssl/openssl/crypto/blake2/blake2b.c"
  "deps/openssl/openssl/crypto/blake2/blake2s.c"
  "deps/openssl/openssl/crypto/blake2/m_blake2b.c"
  "deps/openssl/openssl/crypto/blake2/m_blake2s.c"
  "deps/openssl/openssl/crypto/bn/asm/x86_64-gcc.c"
  "deps/openssl/openssl/crypto/bn/bn_add.c"
  "deps/openssl/openssl/crypto/bn/bn_blind.c"
  "deps/openssl/openssl/crypto/bn/bn_const.c"
  "deps/openssl/openssl/crypto/bn/bn_ctx.c"
  "deps/openssl/openssl/crypto/bn/bn_depr.c"
  "deps/openssl/openssl/crypto/bn/bn_dh.c"
  "deps/openssl/openssl/crypto/bn/bn_div.c"
  "deps/openssl/openssl/crypto/bn/bn_err.c"
  "deps/openssl/openssl/crypto/bn/bn_exp.c"
  "deps/openssl/openssl/crypto/bn/bn_exp2.c"
  "deps/openssl/openssl/crypto/bn/bn_gcd.c"
  "deps/openssl/openssl/crypto/bn/bn_gf2m.c"
  "deps/openssl/openssl/crypto/bn/bn_intern.c"
  "deps/openssl/openssl/crypto/bn/bn_kron.c"
  "deps/openssl/openssl/crypto/bn/bn_lib.c"
  "deps/openssl/openssl/crypto/bn/bn_mod.c"
  "deps/openssl/openssl/crypto/bn/bn_mont.c"
  "deps/openssl/openssl/crypto/bn/bn_mpi.c"
  "deps/openssl/openssl/crypto/bn/bn_mul.c"
  "deps/openssl/openssl/crypto/bn/bn_nist.c"
  "deps/openssl/openssl/crypto/bn/bn_prime.c"
  "deps/openssl/openssl/crypto/bn/bn_print.c"
  "deps/openssl/openssl/crypto/bn/bn_rand.c"
  "deps/openssl/openssl/crypto/bn/bn_recp.c"
  "deps/openssl/openssl/crypto/bn/bn_shift.c"
  "deps/openssl/openssl/crypto/bn/bn_sqr.c"
  "deps/openssl/openssl/crypto/bn/bn_sqrt.c"
  "deps/openssl/openssl/crypto/bn/bn_srp.c"
  "deps/openssl/openssl/crypto/bn/bn_word.c"
  "deps/openssl/openssl/crypto/bn/bn_x931p.c"
  "deps/openssl/openssl/crypto/bn/rsaz_exp.c"
  "deps/openssl/openssl/crypto/buffer/buf_err.c"
  "deps/openssl/openssl/crypto/buffer/buffer.c"
  "deps/openssl/openssl/crypto/camellia/cmll_cfb.c"
  "deps/openssl/openssl/crypto/camellia/cmll_ctr.c"
  "deps/openssl/openssl/crypto/camellia/cmll_ecb.c"
  "deps/openssl/openssl/crypto/camellia/cmll_misc.c"
  "deps/openssl/openssl/crypto/camellia/cmll_ofb.c"
  "deps/openssl/openssl/crypto/cast/c_cfb64.c"
  "deps/openssl/openssl/crypto/cast/c_ecb.c"
  "deps/openssl/openssl/crypto/cast/c_enc.c"
  "deps/openssl/openssl/crypto/cast/c_ofb64.c"
  "deps/openssl/openssl/crypto/cast/c_skey.c"
  "deps/openssl/openssl/crypto/cmac/cm_ameth.c"
  "deps/openssl/openssl/crypto/cmac/cm_pmeth.c"
  "deps/openssl/openssl/crypto/cmac/cmac.c"
  "deps/openssl/openssl/crypto/cms/cms_asn1.c"
  "deps/openssl/openssl/crypto/cms/cms_att.c"
  "deps/openssl/openssl/crypto/cms/cms_cd.c"
  "deps/openssl/openssl/crypto/cms/cms_dd.c"
  "deps/openssl/openssl/crypto/cms/cms_enc.c"
  "deps/openssl/openssl/crypto/cms/cms_env.c"
  "deps/openssl/openssl/crypto/cms/cms_err.c"
  "deps/openssl/openssl/crypto/cms/cms_ess.c"
  "deps/openssl/openssl/crypto/cms/cms_io.c"
  "deps/openssl/openssl/crypto/cms/cms_kari.c"
  "deps/openssl/openssl/crypto/cms/cms_lib.c"
  "deps/openssl/openssl/crypto/cms/cms_pwri.c"
  "deps/openssl/openssl/crypto/cms/cms_sd.c"
  "deps/openssl/openssl/crypto/cms/cms_smime.c"
  "deps/openssl/openssl/crypto/conf/conf_api.c"
  "deps/openssl/openssl/crypto/conf/conf_def.c"
  "deps/openssl/openssl/crypto/conf/conf_err.c"
  "deps/openssl/openssl/crypto/conf/conf_lib.c"
  "deps/openssl/openssl/crypto/conf/conf_mall.c"
  "deps/openssl/openssl/crypto/conf/conf_mod.c"
  "deps/openssl/openssl/crypto/conf/conf_sap.c"
  "deps/openssl/openssl/crypto/conf/conf_ssl.c"
  "deps/openssl/openssl/crypto/cpt_err.c"
  "deps/openssl/openssl/crypto/cryptlib.c"
  "deps/openssl/openssl/crypto/ct/ct_b64.c"
  "deps/openssl/openssl/crypto/ct/ct_err.c"
  "deps/openssl/openssl/crypto/ct/ct_log.c"
  "deps/openssl/openssl/crypto/ct/ct_oct.c"
  "deps/openssl/openssl/crypto/ct/ct_policy.c"
  "deps/openssl/openssl/crypto/ct/ct_prn.c"
  "deps/openssl/openssl/crypto/ct/ct_sct.c"
  "deps/openssl/openssl/crypto/ct/ct_sct_ctx.c"
  "deps/openssl/openssl/crypto/ct/ct_vfy.c"
  "deps/openssl/openssl/crypto/ct/ct_x509v3.c"
  "deps/openssl/openssl/crypto/ctype.c"
  "deps/openssl/openssl/crypto/cversion.c"
  "deps/openssl/openssl/crypto/des/cbc_cksm.c"
  "deps/openssl/openssl/crypto/des/cbc_enc.c"
  "deps/openssl/openssl/crypto/des/cfb64ede.c"
  "deps/openssl/openssl/crypto/des/cfb64enc.c"
  "deps/openssl/openssl/crypto/des/cfb_enc.c"
  "deps/openssl/openssl/crypto/des/des_enc.c"
  "deps/openssl/openssl/crypto/des/ecb3_enc.c"
  "deps/openssl/openssl/crypto/des/ecb_enc.c"
  "deps/openssl/openssl/crypto/des/fcrypt.c"
  "deps/openssl/openssl/crypto/des/fcrypt_b.c"
  "deps/openssl/openssl/crypto/des/ofb64ede.c"
  "deps/openssl/openssl/crypto/des/ofb64enc.c"
  "deps/openssl/openssl/crypto/des/ofb_enc.c"
  "deps/openssl/openssl/crypto/des/pcbc_enc.c"
  "deps/openssl/openssl/crypto/des/qud_cksm.c"
  "deps/openssl/openssl/crypto/des/rand_key.c"
  "deps/openssl/openssl/crypto/des/set_key.c"
  "deps/openssl/openssl/crypto/des/str2key.c"
  "deps/openssl/openssl/crypto/des/xcbc_enc.c"
  "deps/openssl/openssl/crypto/dh/dh_ameth.c"
  "deps/openssl/openssl/crypto/dh/dh_asn1.c"
  "deps/openssl/openssl/crypto/dh/dh_check.c"
  "deps/openssl/openssl/crypto/dh/dh_depr.c"
  "deps/openssl/openssl/crypto/dh/dh_err.c"
  "deps/openssl/openssl/crypto/dh/dh_gen.c"
  "deps/openssl/openssl/crypto/dh/dh_kdf.c"
  "deps/openssl/openssl/crypto/dh/dh_key.c"
  "deps/openssl/openssl/crypto/dh/dh_lib.c"
  "deps/openssl/openssl/crypto/dh/dh_meth.c"
  "deps/openssl/openssl/crypto/dh/dh_pmeth.c"
  "deps/openssl/openssl/crypto/dh/dh_prn.c"
  "deps/openssl/openssl/crypto/dh/dh_rfc5114.c"
  "deps/openssl/openssl/crypto/dh/dh_rfc7919.c"
  "deps/openssl/openssl/crypto/dsa/dsa_ameth.c"
  "deps/openssl/openssl/crypto/dsa/dsa_asn1.c"
  "deps/openssl/openssl/crypto/dsa/dsa_depr.c"
  "deps/openssl/openssl/crypto/dsa/dsa_err.c"
  "deps/openssl/openssl/crypto/dsa/dsa_gen.c"
  "deps/openssl/openssl/crypto/dsa/dsa_key.c"
  "deps/openssl/openssl/crypto/dsa/dsa_lib.c"
  "deps/openssl/openssl/crypto/dsa/dsa_meth.c"
  "deps/openssl/openssl/crypto/dsa/dsa_ossl.c"
  "deps/openssl/openssl/crypto/dsa/dsa_pmeth.c"
  "deps/openssl/openssl/crypto/dsa/dsa_prn.c"
  "deps/openssl/openssl/crypto/dsa/dsa_sign.c"
  "deps/openssl/openssl/crypto/dsa/dsa_vrf.c"
  "deps/openssl/openssl/crypto/dso/dso_dl.c"
  "deps/openssl/openssl/crypto/dso/dso_dlfcn.c"
  "deps/openssl/openssl/crypto/dso/dso_err.c"
  "deps/openssl/openssl/crypto/dso/dso_lib.c"
  "deps/openssl/openssl/crypto/dso/dso_openssl.c"
  "deps/openssl/openssl/crypto/dso/dso_vms.c"
  "deps/openssl/openssl/crypto/dso/dso_win32.c"
  "deps/openssl/openssl/crypto/ebcdic.c"
  "deps/openssl/openssl/crypto/ec/curve25519.c"
  "deps/openssl/openssl/crypto/ec/curve448/arch_32/f_impl.c"
  "deps/openssl/openssl/crypto/ec/curve448/curve448.c"
  "deps/openssl/openssl/crypto/ec/curve448/curve448_tables.c"
  "deps/openssl/openssl/crypto/ec/curve448/eddsa.c"
  "deps/openssl/openssl/crypto/ec/curve448/f_generic.c"
  "deps/openssl/openssl/crypto/ec/curve448/scalar.c"
  "deps/openssl/openssl/crypto/ec/ec2_oct.c"
  "deps/openssl/openssl/crypto/ec/ec2_smpl.c"
  "deps/openssl/openssl/crypto/ec/ec_ameth.c"
  "deps/openssl/openssl/crypto/ec/ec_asn1.c"
  "deps/openssl/openssl/crypto/ec/ec_check.c"
  "deps/openssl/openssl/crypto/ec/ec_curve.c"
  "deps/openssl/openssl/crypto/ec/ec_cvt.c"
  "deps/openssl/openssl/crypto/ec/ec_err.c"
  "deps/openssl/openssl/crypto/ec/ec_key.c"
  "deps/openssl/openssl/crypto/ec/ec_kmeth.c"
  "deps/openssl/openssl/crypto/ec/ec_lib.c"
  "deps/openssl/openssl/crypto/ec/ec_mult.c"
  "deps/openssl/openssl/crypto/ec/ec_oct.c"
  "deps/openssl/openssl/crypto/ec/ec_pmeth.c"
  "deps/openssl/openssl/crypto/ec/ec_print.c"
  "deps/openssl/openssl/crypto/ec/ecdh_kdf.c"
  "deps/openssl/openssl/crypto/ec/ecdh_ossl.c"
  "deps/openssl/openssl/crypto/ec/ecdsa_ossl.c"
  "deps/openssl/openssl/crypto/ec/ecdsa_sign.c"
  "deps/openssl/openssl/crypto/ec/ecdsa_vrf.c"
  "deps/openssl/openssl/crypto/ec/eck_prn.c"
  "deps/openssl/openssl/crypto/ec/ecp_mont.c"
  "deps/openssl/openssl/crypto/ec/ecp_nist.c"
  "deps/openssl/openssl/crypto/ec/ecp_nistp224.c"
  "deps/openssl/openssl/crypto/ec/ecp_nistp256.c"
  "deps/openssl/openssl/crypto/ec/ecp_nistp521.c"
  "deps/openssl/openssl/crypto/ec/ecp_nistputil.c"
  "deps/openssl/openssl/crypto/ec/ecp_nistz256.c"
  "deps/openssl/openssl/crypto/ec/ecp_oct.c"
  "deps/openssl/openssl/crypto/ec/ecp_smpl.c"
  "deps/openssl/openssl/crypto/ec/ecx_meth.c"
  "deps/openssl/openssl/crypto/engine/eng_all.c"
  "deps/openssl/openssl/crypto/engine/eng_cnf.c"
  "deps/openssl/openssl/crypto/engine/eng_ctrl.c"
  "deps/openssl/openssl/crypto/engine/eng_dyn.c"
  "deps/openssl/openssl/crypto/engine/eng_err.c"
  "deps/openssl/openssl/crypto/engine/eng_fat.c"
  "deps/openssl/openssl/crypto/engine/eng_init.c"
  "deps/openssl/openssl/crypto/engine/eng_lib.c"
  "deps/openssl/openssl/crypto/engine/eng_list.c"
  "deps/openssl/openssl/crypto/engine/eng_openssl.c"
  "deps/openssl/openssl/crypto/engine/eng_pkey.c"
  "deps/openssl/openssl/crypto/engine/eng_rdrand.c"
  "deps/openssl/openssl/crypto/engine/eng_table.c"
  "deps/openssl/openssl/crypto/engine/tb_asnmth.c"
  "deps/openssl/openssl/crypto/engine/tb_cipher.c"
  "deps/openssl/openssl/crypto/engine/tb_dh.c"
  "deps/openssl/openssl/crypto/engine/tb_digest.c"
  "deps/openssl/openssl/crypto/engine/tb_dsa.c"
  "deps/openssl/openssl/crypto/engine/tb_eckey.c"
  "deps/openssl/openssl/crypto/engine/tb_pkmeth.c"
  "deps/openssl/openssl/crypto/engine/tb_rand.c"
  "deps/openssl/openssl/crypto/engine/tb_rsa.c"
  "deps/openssl/openssl/crypto/err/err.c"
  "deps/openssl/openssl/crypto/err/err_all.c"
  "deps/openssl/openssl/crypto/err/err_prn.c"
  "deps/openssl/openssl/crypto/evp/bio_b64.c"
  "deps/openssl/openssl/crypto/evp/bio_enc.c"
  "deps/openssl/openssl/crypto/evp/bio_md.c"
  "deps/openssl/openssl/crypto/evp/bio_ok.c"
  "deps/openssl/openssl/crypto/evp/c_allc.c"
  "deps/openssl/openssl/crypto/evp/c_alld.c"
  "deps/openssl/openssl/crypto/evp/cmeth_lib.c"
  "deps/openssl/openssl/crypto/evp/digest.c"
  "deps/openssl/openssl/crypto/evp/e_aes.c"
  "deps/openssl/openssl/crypto/evp/e_aes_cbc_hmac_sha1.c"
  "deps/openssl/openssl/crypto/evp/e_aes_cbc_hmac_sha256.c"
  "deps/openssl/openssl/crypto/evp/e_aria.c"
  "deps/openssl/openssl/crypto/evp/e_bf.c"
  "deps/openssl/openssl/crypto/evp/e_camellia.c"
  "deps/openssl/openssl/crypto/evp/e_cast.c"
  "deps/openssl/openssl/crypto/evp/e_chacha20_poly1305.c"
  "deps/openssl/openssl/crypto/evp/e_des.c"
  "deps/openssl/openssl/crypto/evp/e_des3.c"
  "deps/openssl/openssl/crypto/evp/e_idea.c"
  "deps/openssl/openssl/crypto/evp/e_null.c"
  "deps/openssl/openssl/crypto/evp/e_old.c"
  "deps/openssl/openssl/crypto/evp/e_rc2.c"
  "deps/openssl/openssl/crypto/evp/e_rc4.c"
  "deps/openssl/openssl/crypto/evp/e_rc4_hmac_md5.c"
  "deps/openssl/openssl/crypto/evp/e_rc5.c"
  "deps/openssl/openssl/crypto/evp/e_seed.c"
  "deps/openssl/openssl/crypto/evp/e_sm4.c"
  "deps/openssl/openssl/crypto/evp/e_xcbc_d.c"
  "deps/openssl/openssl/crypto/evp/encode.c"
  "deps/openssl/openssl/crypto/evp/evp_cnf.c"
  "deps/openssl/openssl/crypto/evp/evp_enc.c"
  "deps/openssl/openssl/crypto/evp/evp_err.c"
  "deps/openssl/openssl/crypto/evp/evp_key.c"
  "deps/openssl/openssl/crypto/evp/evp_lib.c"
  "deps/openssl/openssl/crypto/evp/evp_pbe.c"
  "deps/openssl/openssl/crypto/evp/evp_pkey.c"
  "deps/openssl/openssl/crypto/evp/m_md2.c"
  "deps/openssl/openssl/crypto/evp/m_md4.c"
  "deps/openssl/openssl/crypto/evp/m_md5.c"
  "deps/openssl/openssl/crypto/evp/m_md5_sha1.c"
  "deps/openssl/openssl/crypto/evp/m_mdc2.c"
  "deps/openssl/openssl/crypto/evp/m_null.c"
  "deps/openssl/openssl/crypto/evp/m_ripemd.c"
  "deps/openssl/openssl/crypto/evp/m_sha1.c"
  "deps/openssl/openssl/crypto/evp/m_sha3.c"
  "deps/openssl/openssl/crypto/evp/m_sigver.c"
  "deps/openssl/openssl/crypto/evp/m_wp.c"
  "deps/openssl/openssl/crypto/evp/names.c"
  "deps/openssl/openssl/crypto/evp/p5_crpt.c"
  "deps/openssl/openssl/crypto/evp/p5_crpt2.c"
  "deps/openssl/openssl/crypto/evp/p_dec.c"
  "deps/openssl/openssl/crypto/evp/p_enc.c"
  "deps/openssl/openssl/crypto/evp/p_lib.c"
  "deps/openssl/openssl/crypto/evp/p_open.c"
  "deps/openssl/openssl/crypto/evp/p_seal.c"
  "deps/openssl/openssl/crypto/evp/p_sign.c"
  "deps/openssl/openssl/crypto/evp/p_verify.c"
  "deps/openssl/openssl/crypto/evp/pbe_scrypt.c"
  "deps/openssl/openssl/crypto/evp/pmeth_fn.c"
  "deps/openssl/openssl/crypto/evp/pmeth_gn.c"
  "deps/openssl/openssl/crypto/evp/pmeth_lib.c"
  "deps/openssl/openssl/crypto/ex_data.c"
  "deps/openssl/openssl/crypto/getenv.c"
  "deps/openssl/openssl/crypto/hmac/hm_ameth.c"
  "deps/openssl/openssl/crypto/hmac/hm_pmeth.c"
  "deps/openssl/openssl/crypto/hmac/hmac.c"
  "deps/openssl/openssl/crypto/idea/i_cbc.c"
  "deps/openssl/openssl/crypto/idea/i_cfb64.c"
  "deps/openssl/openssl/crypto/idea/i_ecb.c"
  "deps/openssl/openssl/crypto/idea/i_ofb64.c"
  "deps/openssl/openssl/crypto/idea/i_skey.c"
  "deps/openssl/openssl/crypto/init.c"
  "deps/openssl/openssl/crypto/kdf/hkdf.c"
  "deps/openssl/openssl/crypto/kdf/kdf_err.c"
  "deps/openssl/openssl/crypto/kdf/scrypt.c"
  "deps/openssl/openssl/crypto/kdf/tls1_prf.c"
  "deps/openssl/openssl/crypto/lhash/lh_stats.c"
  "deps/openssl/openssl/crypto/lhash/lhash.c"
  "deps/openssl/openssl/crypto/md4/md4_dgst.c"
  "deps/openssl/openssl/crypto/md4/md4_one.c"
  "deps/openssl/openssl/crypto/md5/md5_dgst.c"
  "deps/openssl/openssl/crypto/md5/md5_one.c"
  "deps/openssl/openssl/crypto/mdc2/mdc2_one.c"
  "deps/openssl/openssl/crypto/mdc2/mdc2dgst.c"
  "deps/openssl/openssl/crypto/mem.c"
  "deps/openssl/openssl/crypto/mem_dbg.c"
  "deps/openssl/openssl/crypto/mem_sec.c"
  "deps/openssl/openssl/crypto/modes/cbc128.c"
  "deps/openssl/openssl/crypto/modes/ccm128.c"
  "deps/openssl/openssl/crypto/modes/cfb128.c"
  "deps/openssl/openssl/crypto/modes/ctr128.c"
  "deps/openssl/openssl/crypto/modes/cts128.c"
  "deps/openssl/openssl/crypto/modes/gcm128.c"
  "deps/openssl/openssl/crypto/modes/ocb128.c"
  "deps/openssl/openssl/crypto/modes/ofb128.c"
  "deps/openssl/openssl/crypto/modes/wrap128.c"
  "deps/openssl/openssl/crypto/modes/xts128.c"
  "deps/openssl/openssl/crypto/o_dir.c"
  "deps/openssl/openssl/crypto/o_fips.c"
  "deps/openssl/openssl/crypto/o_fopen.c"
  "deps/openssl/openssl/crypto/o_init.c"
  "deps/openssl/openssl/crypto/o_str.c"
  "deps/openssl/openssl/crypto/o_time.c"
  "deps/openssl/openssl/crypto/objects/o_names.c"
  "deps/openssl/openssl/crypto/objects/obj_dat.c"
  "deps/openssl/openssl/crypto/objects/obj_err.c"
  "deps/openssl/openssl/crypto/objects/obj_lib.c"
  "deps/openssl/openssl/crypto/objects/obj_xref.c"
  "deps/openssl/openssl/crypto/ocsp/ocsp_asn.c"
  "deps/openssl/openssl/crypto/ocsp/ocsp_cl.c"
  "deps/openssl/openssl/crypto/ocsp/ocsp_err.c"
  "deps/openssl/openssl/crypto/ocsp/ocsp_ext.c"
  "deps/openssl/openssl/crypto/ocsp/ocsp_ht.c"
  "deps/openssl/openssl/crypto/ocsp/ocsp_lib.c"
  "deps/openssl/openssl/crypto/ocsp/ocsp_prn.c"
  "deps/openssl/openssl/crypto/ocsp/ocsp_srv.c"
  "deps/openssl/openssl/crypto/ocsp/ocsp_vfy.c"
  "deps/openssl/openssl/crypto/ocsp/v3_ocsp.c"
  "deps/openssl/openssl/crypto/pem/pem_all.c"
  "deps/openssl/openssl/crypto/pem/pem_err.c"
  "deps/openssl/openssl/crypto/pem/pem_info.c"
  "deps/openssl/openssl/crypto/pem/pem_lib.c"
  "deps/openssl/openssl/crypto/pem/pem_oth.c"
  "deps/openssl/openssl/crypto/pem/pem_pk8.c"
  "deps/openssl/openssl/crypto/pem/pem_pkey.c"
  "deps/openssl/openssl/crypto/pem/pem_sign.c"
  "deps/openssl/openssl/crypto/pem/pem_x509.c"
  "deps/openssl/openssl/crypto/pem/pem_xaux.c"
  "deps/openssl/openssl/crypto/pem/pvkfmt.c"
  "deps/openssl/openssl/crypto/pkcs12/p12_add.c"
  "deps/openssl/openssl/crypto/pkcs12/p12_asn.c"
  "deps/openssl/openssl/crypto/pkcs12/p12_attr.c"
  "deps/openssl/openssl/crypto/pkcs12/p12_crpt.c"
  "deps/openssl/openssl/crypto/pkcs12/p12_crt.c"
  "deps/openssl/openssl/crypto/pkcs12/p12_decr.c"
  "deps/openssl/openssl/crypto/pkcs12/p12_init.c"
  "deps/openssl/openssl/crypto/pkcs12/p12_key.c"
  "deps/openssl/openssl/crypto/pkcs12/p12_kiss.c"
  "deps/openssl/openssl/crypto/pkcs12/p12_mutl.c"
  "deps/openssl/openssl/crypto/pkcs12/p12_npas.c"
  "deps/openssl/openssl/crypto/pkcs12/p12_p8d.c"
  "deps/openssl/openssl/crypto/pkcs12/p12_p8e.c"
  "deps/openssl/openssl/crypto/pkcs12/p12_sbag.c"
  "deps/openssl/openssl/crypto/pkcs12/p12_utl.c"
  "deps/openssl/openssl/crypto/pkcs12/pk12err.c"
  "deps/openssl/openssl/crypto/pkcs7/bio_pk7.c"
  "deps/openssl/openssl/crypto/pkcs7/pk7_asn1.c"
  "deps/openssl/openssl/crypto/pkcs7/pk7_attr.c"
  "deps/openssl/openssl/crypto/pkcs7/pk7_doit.c"
  "deps/openssl/openssl/crypto/pkcs7/pk7_lib.c"
  "deps/openssl/openssl/crypto/pkcs7/pk7_mime.c"
  "deps/openssl/openssl/crypto/pkcs7/pk7_smime.c"
  "deps/openssl/openssl/crypto/pkcs7/pkcs7err.c"
  "deps/openssl/openssl/crypto/poly1305/poly1305.c"
  "deps/openssl/openssl/crypto/poly1305/poly1305_ameth.c"
  "deps/openssl/openssl/crypto/poly1305/poly1305_pmeth.c"
  "deps/openssl/openssl/crypto/rand/drbg_ctr.c"
  "deps/openssl/openssl/crypto/rand/drbg_lib.c"
  "deps/openssl/openssl/crypto/rand/rand_egd.c"
  "deps/openssl/openssl/crypto/rand/rand_err.c"
  "deps/openssl/openssl/crypto/rand/rand_lib.c"
  "deps/openssl/openssl/crypto/rand/rand_unix.c"
  "deps/openssl/openssl/crypto/rand/rand_vms.c"
  "deps/openssl/openssl/crypto/rand/rand_win.c"
  "deps/openssl/openssl/crypto/rand/randfile.c"
  "deps/openssl/openssl/crypto/rc2/rc2_cbc.c"
  "deps/openssl/openssl/crypto/rc2/rc2_ecb.c"
  "deps/openssl/openssl/crypto/rc2/rc2_skey.c"
  "deps/openssl/openssl/crypto/rc2/rc2cfb64.c"
  "deps/openssl/openssl/crypto/rc2/rc2ofb64.c"
  "deps/openssl/openssl/crypto/ripemd/rmd_dgst.c"
  "deps/openssl/openssl/crypto/ripemd/rmd_one.c"
  "deps/openssl/openssl/crypto/rsa/rsa_ameth.c"
  "deps/openssl/openssl/crypto/rsa/rsa_asn1.c"
  "deps/openssl/openssl/crypto/rsa/rsa_chk.c"
  "deps/openssl/openssl/crypto/rsa/rsa_crpt.c"
  "deps/openssl/openssl/crypto/rsa/rsa_depr.c"
  "deps/openssl/openssl/crypto/rsa/rsa_err.c"
  "deps/openssl/openssl/crypto/rsa/rsa_gen.c"
  "deps/openssl/openssl/crypto/rsa/rsa_lib.c"
  "deps/openssl/openssl/crypto/rsa/rsa_meth.c"
  "deps/openssl/openssl/crypto/rsa/rsa_mp.c"
  "deps/openssl/openssl/crypto/rsa/rsa_none.c"
  "deps/openssl/openssl/crypto/rsa/rsa_oaep.c"
  "deps/openssl/openssl/crypto/rsa/rsa_ossl.c"
  "deps/openssl/openssl/crypto/rsa/rsa_pk1.c"
  "deps/openssl/openssl/crypto/rsa/rsa_pmeth.c"
  "deps/openssl/openssl/crypto/rsa/rsa_prn.c"
  "deps/openssl/openssl/crypto/rsa/rsa_pss.c"
  "deps/openssl/openssl/crypto/rsa/rsa_saos.c"
  "deps/openssl/openssl/crypto/rsa/rsa_sign.c"
  "deps/openssl/openssl/crypto/rsa/rsa_ssl.c"
  "deps/openssl/openssl/crypto/rsa/rsa_x931.c"
  "deps/openssl/openssl/crypto/rsa/rsa_x931g.c"
  "deps/openssl/openssl/crypto/seed/seed.c"
  "deps/openssl/openssl/crypto/seed/seed_cbc.c"
  "deps/openssl/openssl/crypto/seed/seed_cfb.c"
  "deps/openssl/openssl/crypto/seed/seed_ecb.c"
  "deps/openssl/openssl/crypto/seed/seed_ofb.c"
  "deps/openssl/openssl/crypto/sha/sha1_one.c"
  "deps/openssl/openssl/crypto/sha/sha1dgst.c"
  "deps/openssl/openssl/crypto/sha/sha256.c"
  "deps/openssl/openssl/crypto/sha/sha512.c"
  "deps/openssl/openssl/crypto/siphash/siphash.c"
  "deps/openssl/openssl/crypto/siphash/siphash_ameth.c"
  "deps/openssl/openssl/crypto/siphash/siphash_pmeth.c"
  "deps/openssl/openssl/crypto/sm2/sm2_crypt.c"
  "deps/openssl/openssl/crypto/sm2/sm2_err.c"
  "deps/openssl/openssl/crypto/sm2/sm2_pmeth.c"
  "deps/openssl/openssl/crypto/sm2/sm2_sign.c"
  "deps/openssl/openssl/crypto/sm3/m_sm3.c"
  "deps/openssl/openssl/crypto/sm3/sm3.c"
  "deps/openssl/openssl/crypto/sm4/sm4.c"
  "deps/openssl/openssl/crypto/srp/srp_lib.c"
  "deps/openssl/openssl/crypto/srp/srp_vfy.c"
  "deps/openssl/openssl/crypto/stack/stack.c"
  "deps/openssl/openssl/crypto/store/loader_file.c"
  "deps/openssl/openssl/crypto/store/store_err.c"
  "deps/openssl/openssl/crypto/store/store_init.c"
  "deps/openssl/openssl/crypto/store/store_lib.c"
  "deps/openssl/openssl/crypto/store/store_register.c"
  "deps/openssl/openssl/crypto/store/store_strings.c"
  "deps/openssl/openssl/crypto/threads_none.c"
  "deps/openssl/openssl/crypto/threads_pthread.c"
  "deps/openssl/openssl/crypto/threads_win.c"
  "deps/openssl/openssl/crypto/ts/ts_asn1.c"
  "deps/openssl/openssl/crypto/ts/ts_conf.c"
  "deps/openssl/openssl/crypto/ts/ts_err.c"
  "deps/openssl/openssl/crypto/ts/ts_lib.c"
  "deps/openssl/openssl/crypto/ts/ts_req_print.c"
  "deps/openssl/openssl/crypto/ts/ts_req_utils.c"
  "deps/openssl/openssl/crypto/ts/ts_rsp_print.c"
  "deps/openssl/openssl/crypto/ts/ts_rsp_sign.c"
  "deps/openssl/openssl/crypto/ts/ts_rsp_utils.c"
  "deps/openssl/openssl/crypto/ts/ts_rsp_verify.c"
  "deps/openssl/openssl/crypto/ts/ts_verify_ctx.c"
  "deps/openssl/openssl/crypto/txt_db/txt_db.c"
  "deps/openssl/openssl/crypto/ui/ui_err.c"
  "deps/openssl/openssl/crypto/ui/ui_lib.c"
  "deps/openssl/openssl/crypto/ui/ui_null.c"
  "deps/openssl/openssl/crypto/ui/ui_openssl.c"
  "deps/openssl/openssl/crypto/ui/ui_util.c"
  "deps/openssl/openssl/crypto/uid.c"
  "deps/openssl/openssl/crypto/whrlpool/wp_dgst.c"
  "deps/openssl/openssl/crypto/x509/by_dir.c"
  "deps/openssl/openssl/crypto/x509/by_file.c"
  "deps/openssl/openssl/crypto/x509/t_crl.c"
  "deps/openssl/openssl/crypto/x509/t_req.c"
  "deps/openssl/openssl/crypto/x509/t_x509.c"
  "deps/openssl/openssl/crypto/x509/x509_att.c"
  "deps/openssl/openssl/crypto/x509/x509_cmp.c"
  "deps/openssl/openssl/crypto/x509/x509_d2.c"
  "deps/openssl/openssl/crypto/x509/x509_def.c"
  "deps/openssl/openssl/crypto/x509/x509_err.c"
  "deps/openssl/openssl/crypto/x509/x509_ext.c"
  "deps/openssl/openssl/crypto/x509/x509_lu.c"
  "deps/openssl/openssl/crypto/x509/x509_meth.c"
  "deps/openssl/openssl/crypto/x509/x509_obj.c"
  "deps/openssl/openssl/crypto/x509/x509_r2x.c"
  "deps/openssl/openssl/crypto/x509/x509_req.c"
  "deps/openssl/openssl/crypto/x509/x509_set.c"
  "deps/openssl/openssl/crypto/x509/x509_trs.c"
  "deps/openssl/openssl/crypto/x509/x509_txt.c"
  "deps/openssl/openssl/crypto/x509/x509_v3.c"
  "deps/openssl/openssl/crypto/x509/x509_vfy.c"
  "deps/openssl/openssl/crypto/x509/x509_vpm.c"
  "deps/openssl/openssl/crypto/x509/x509cset.c"
  "deps/openssl/openssl/crypto/x509/x509name.c"
  "deps/openssl/openssl/crypto/x509/x509rset.c"
  "deps/openssl/openssl/crypto/x509/x509spki.c"
  "deps/openssl/openssl/crypto/x509/x509type.c"
  "deps/openssl/openssl/crypto/x509/x_all.c"
  "deps/openssl/openssl/crypto/x509/x_attrib.c"
  "deps/openssl/openssl/crypto/x509/x_crl.c"
  "deps/openssl/openssl/crypto/x509/x_exten.c"
  "deps/openssl/openssl/crypto/x509/x_name.c"
  "deps/openssl/openssl/crypto/x509/x_pubkey.c"
  "deps/openssl/openssl/crypto/x509/x_req.c"
  "deps/openssl/openssl/crypto/x509/x_x509.c"
  "deps/openssl/openssl/crypto/x509/x_x509a.c"
  "deps/openssl/openssl/crypto/x509v3/pcy_cache.c"
  "deps/openssl/openssl/crypto/x509v3/pcy_data.c"
  "deps/openssl/openssl/crypto/x509v3/pcy_lib.c"
  "deps/openssl/openssl/crypto/x509v3/pcy_map.c"
  "deps/openssl/openssl/crypto/x509v3/pcy_node.c"
  "deps/openssl/openssl/crypto/x509v3/pcy_tree.c"
  "deps/openssl/openssl/crypto/x509v3/v3_addr.c"
  "deps/openssl/openssl/crypto/x509v3/v3_admis.c"
  "deps/openssl/openssl/crypto/x509v3/v3_akey.c"
  "deps/openssl/openssl/crypto/x509v3/v3_akeya.c"
  "deps/openssl/openssl/crypto/x509v3/v3_alt.c"
  "deps/openssl/openssl/crypto/x509v3/v3_asid.c"
  "deps/openssl/openssl/crypto/x509v3/v3_bcons.c"
  "deps/openssl/openssl/crypto/x509v3/v3_bitst.c"
  "deps/openssl/openssl/crypto/x509v3/v3_conf.c"
  "deps/openssl/openssl/crypto/x509v3/v3_cpols.c"
  "deps/openssl/openssl/crypto/x509v3/v3_crld.c"
  "deps/openssl/openssl/crypto/x509v3/v3_enum.c"
  "deps/openssl/openssl/crypto/x509v3/v3_extku.c"
  "deps/openssl/openssl/crypto/x509v3/v3_genn.c"
  "deps/openssl/openssl/crypto/x509v3/v3_ia5.c"
  "deps/openssl/openssl/crypto/x509v3/v3_info.c"
  "deps/openssl/openssl/crypto/x509v3/v3_int.c"
  "deps/openssl/openssl/crypto/x509v3/v3_lib.c"
  "deps/openssl/openssl/crypto/x509v3/v3_ncons.c"
  "deps/openssl/openssl/crypto/x509v3/v3_pci.c"
  "deps/openssl/openssl/crypto/x509v3/v3_pcia.c"
  "deps/openssl/openssl/crypto/x509v3/v3_pcons.c"
  "deps/openssl/openssl/crypto/x509v3/v3_pku.c"
  "deps/openssl/openssl/crypto/x509v3/v3_pmaps.c"
  "deps/openssl/openssl/crypto/x509v3/v3_prn.c"
  "deps/openssl/openssl/crypto/x509v3/v3_purp.c"
  "deps/openssl/openssl/crypto/x509v3/v3_skey.c"
  "deps/openssl/openssl/crypto/x509v3/v3_sxnet.c"
  "deps/openssl/openssl/crypto/x509v3/v3_tlsf.c"
  "deps/openssl/openssl/crypto/x509v3/v3_utl.c"
  "deps/openssl/openssl/crypto/x509v3/v3err.c"
  "deps/openssl/openssl/engines/e_capi.c"
  "deps/openssl/openssl/engines/e_padlock.c")
add_library(openssl STATIC ${openssl__asm_srcs} ${openssl__c_srcs})
set_target_properties(openssl PROPERTIES EXCLUDE_FROM_ALL "FALSE")
set_target_properties(openssl PROPERTIES ARCHIVE_OUTPUT_DIRECTORY "${builddir}")
set_target_properties(openssl PROPERTIES PREFIX "lib")
set_target_properties(openssl PROPERTIES ARCHIVE_OUTPUT_NAME "openssl")
set_target_properties(openssl PROPERTIES SUFFIX ".a")
set_source_files_properties(${builddir}/libopenssl.a PROPERTIES GENERATED "TRUE")
list(APPEND openssl__include_dirs
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/include"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/crypto"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/crypto/include"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/crypto/modes"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/crypto/ec/curve448"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/crypto/ec/curve448/arch_32"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/config"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/config/archs/linux-x86_64/asm_avx2"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/config/archs/linux-x86_64/asm_avx2/include"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/config/archs/linux-x86_64/asm_avx2/crypto/include/internal")
set_property(TARGET openssl APPEND PROPERTY INCLUDE_DIRECTORIES ${openssl__include_dirs})
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(openssl PROPERTIES COMPILE_DEFINITIONS "OPENSSL_NO_HW;NDEBUG;OPENSSL_USE_NODELETE;L_ENDIAN;OPENSSL_PIC;OPENSSL_CPUID_OBJ;OPENSSL_IA32_SSE2;OPENSSL_BN_ASM_MONT;OPENSSL_BN_ASM_MONT5;OPENSSL_BN_ASM_GF2m;SHA1_ASM;SHA256_ASM;SHA512_ASM;KECCAK1600_ASM;RC4_ASM;MD5_ASM;AESNI_ASM;VPAES_ASM;GHASH_ASM;ECP_NISTZ256_ASM;X25519_ASM;POLY1305_ASM;OPENSSLDIR=\"/etc/ssl\";ENGINESDIR=\"/dev/null\";TERMIOS;")
  set_target_properties(openssl PROPERTIES COMPILE_FLAGS "-m64 -Wa,--noexecstack -Wall -O3 -pthread -m64 -Wall -O3 -Wno-missing-field-initializers -Wno-old-style-declaration -O3 -Wno-unknown-warning-option -fPIC ")
elseif(${CMAKE_BUILD_TYPE} MATCHES "Release")
  set_target_properties(openssl PROPERTIES COMPILE_DEFINITIONS "OPENSSL_NO_HW;NDEBUG;OPENSSL_USE_NODELETE;L_ENDIAN;OPENSSL_PIC;OPENSSL_CPUID_OBJ;OPENSSL_IA32_SSE2;OPENSSL_BN_ASM_MONT;OPENSSL_BN_ASM_MONT5;OPENSSL_BN_ASM_GF2m;SHA1_ASM;SHA256_ASM;SHA512_ASM;KECCAK1600_ASM;RC4_ASM;MD5_ASM;AESNI_ASM;VPAES_ASM;GHASH_ASM;ECP_NISTZ256_ASM;X25519_ASM;POLY1305_ASM;OPENSSLDIR=\"/etc/ssl\";ENGINESDIR=\"/dev/null\";TERMIOS;DEBUG;MS_LOG_TRACE;MS_LOG_FILE_LINE;")
  set_target_properties(openssl PROPERTIES COMPILE_FLAGS "-m64 -Wa,--noexecstack -Wall -O3 -pthread -m64 -Wall -O3 -Wno-missing-field-initializers -Wno-old-style-declaration -g -O0 -Wno-parentheses-equality -Wno-unknown-warning-option -fPIC ")
else()
  message(FATAL_ERROR "Build type must be either Debug or Release, \"${CMAKE_BUILD_TYPE}\" was specified")
endif()
set_target_properties(openssl PROPERTIES LINK_FLAGS "-m64 ")
set_target_properties(openssl PROPERTIES XCODE_ATTRIBUTE_GCC_VERSION "com.apple.compilers.llvm.clang.1_0")
set_target_properties(openssl PROPERTIES XCODE_ATTRIBUTE_GCC_WARN_ABOUT_MISSING_NEWLINE "YES")
set_target_properties(openssl PROPERTIES XCODE_ATTRIBUTE_OTHER_CFLAGS "-fstrict-aliasing -g -fPIC ")
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(openssl PROPERTIES XCODE_ATTRIBUTE_GCC_OPTIMIZATION_LEVEL "0")
endif()
set_target_properties(openssl PROPERTIES XCODE_ATTRIBUTE_PREBINDING "NO")
set_target_properties(openssl PROPERTIES XCODE_ATTRIBUTE_WARNING_CFLAGS "-Wall -Wendif-labels -W -Wno-unused-parameter -Wundeclared-selector -Wno-parentheses-equality ")
set_target_properties(openssl PROPERTIES XCODE_ATTRIBUTE_ARCHS "x86_64 ")

# usrsctp
set(sctp_werror OFF CACHE INTERNAL "")
set(sctp_build_programs OFF CACHE INTERNAL "")
set(sctp_build_fuzzer OFF CACHE INTERNAL "")
set(sctp_link_programs_static OFF CACHE INTERNAL "")
add_subdirectory(deps/usrsctp/usrsctp)
unset(sctp_werror)
unset(sctp_build_programs)
unset(sctp_build_fuzzer)
unset(sctp_link_programs_static)

# json
include_directories(deps/json/single_include/nlohmann)

#mediasoup/worker/deps/openssl/openssl.gyp:openssl-cli#target
list(APPEND openssl-cli__c_srcs
  "deps/openssl/openssl/apps/asn1pars.c"
  "deps/openssl/openssl/apps/ca.c"
  "deps/openssl/openssl/apps/ciphers.c"
  "deps/openssl/openssl/apps/cms.c"
  "deps/openssl/openssl/apps/crl.c"
  "deps/openssl/openssl/apps/crl2p7.c"
  "deps/openssl/openssl/apps/dgst.c"
  "deps/openssl/openssl/apps/dhparam.c"
  "deps/openssl/openssl/apps/dsa.c"
  "deps/openssl/openssl/apps/dsaparam.c"
  "deps/openssl/openssl/apps/ec.c"
  "deps/openssl/openssl/apps/ecparam.c"
  "deps/openssl/openssl/apps/enc.c"
  "deps/openssl/openssl/apps/engine.c"
  "deps/openssl/openssl/apps/errstr.c"
  "deps/openssl/openssl/apps/gendsa.c"
  "deps/openssl/openssl/apps/genpkey.c"
  "deps/openssl/openssl/apps/genrsa.c"
  "deps/openssl/openssl/apps/nseq.c"
  "deps/openssl/openssl/apps/ocsp.c"
  "deps/openssl/openssl/apps/openssl.c"
  "deps/openssl/openssl/apps/passwd.c"
  "deps/openssl/openssl/apps/pkcs12.c"
  "deps/openssl/openssl/apps/pkcs7.c"
  "deps/openssl/openssl/apps/pkcs8.c"
  "deps/openssl/openssl/apps/pkey.c"
  "deps/openssl/openssl/apps/pkeyparam.c"
  "deps/openssl/openssl/apps/pkeyutl.c"
  "deps/openssl/openssl/apps/prime.c"
  "deps/openssl/openssl/apps/rand.c"
  "deps/openssl/openssl/apps/rehash.c"
  "deps/openssl/openssl/apps/req.c"
  "deps/openssl/openssl/apps/rsa.c"
  "deps/openssl/openssl/apps/rsautl.c"
  "deps/openssl/openssl/apps/s_client.c"
  "deps/openssl/openssl/apps/s_server.c"
  "deps/openssl/openssl/apps/s_time.c"
  "deps/openssl/openssl/apps/sess_id.c"
  "deps/openssl/openssl/apps/smime.c"
  "deps/openssl/openssl/apps/speed.c"
  "deps/openssl/openssl/apps/spkac.c"
  "deps/openssl/openssl/apps/srp.c"
  "deps/openssl/openssl/apps/storeutl.c"
  "deps/openssl/openssl/apps/ts.c"
  "deps/openssl/openssl/apps/verify.c"
  "deps/openssl/openssl/apps/version.c"
  "deps/openssl/openssl/apps/x509.c"
  "deps/openssl/openssl/apps/app_rand.c"
  "deps/openssl/openssl/apps/apps.c"
  "deps/openssl/openssl/apps/bf_prefix.c"
  "deps/openssl/openssl/apps/opt.c"
  "deps/openssl/openssl/apps/s_cb.c"
  "deps/openssl/openssl/apps/s_socket.c")
add_executable(openssl-cli ${openssl-cli__c_srcs})
set_target_properties(openssl-cli PROPERTIES EXCLUDE_FROM_ALL "TRUE")
set_target_properties(openssl-cli PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${builddir}")
set_target_properties(openssl-cli PROPERTIES PREFIX "")
set_target_properties(openssl-cli PROPERTIES RUNTIME_OUTPUT_NAME "openssl-cli")
set_target_properties(openssl-cli PROPERTIES SUFFIX "")
set_source_files_properties(${builddir}/openssl-cli PROPERTIES GENERATED "TRUE")
list(APPEND openssl-cli__include_dirs
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/include"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/crypto"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/crypto/include"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/crypto/modes"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/crypto/ec/curve448"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/crypto/ec/curve448/arch_32"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/config"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/config/archs/linux-x86_64/asm/include"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/include")
set_property(TARGET openssl-cli APPEND PROPERTY INCLUDE_DIRECTORIES ${openssl-cli__include_dirs})
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(openssl-cli PROPERTIES COMPILE_DEFINITIONS "NDEBUG;OPENSSL_USE_NODELETE;L_ENDIAN;OPENSSL_PIC;OPENSSL_CPUID_OBJ;OPENSSL_IA32_SSE2;OPENSSL_BN_ASM_MONT;OPENSSL_BN_ASM_MONT5;OPENSSL_BN_ASM_GF2m;SHA1_ASM;SHA256_ASM;SHA512_ASM;KECCAK1600_ASM;RC4_ASM;MD5_ASM;AESNI_ASM;VPAES_ASM;GHASH_ASM;ECP_NISTZ256_ASM;X25519_ASM;POLY1305_ASM;OPENSSLDIR=\"/etc/ssl\";ENGINESDIR=\"/dev/null\";TERMIOS;DEBUG;MS_LOG_TRACE;MS_LOG_FILE_LINE;")
  set_target_properties(openssl-cli PROPERTIES COMPILE_FLAGS "-m64 -Wa,--noexecstack -Wall -O3 -pthread -m64 -Wall -O3 -Wno-missing-field-initializers -Wno-old-style-declaration -g -O0 -Wno-parentheses-equality -Wno-unknown-warning-option -fPIC ")
elseif(${CMAKE_BUILD_TYPE} MATCHES "Release")
  set_target_properties(openssl-cli PROPERTIES COMPILE_DEFINITIONS "NDEBUG;OPENSSL_USE_NODELETE;L_ENDIAN;OPENSSL_PIC;OPENSSL_CPUID_OBJ;OPENSSL_IA32_SSE2;OPENSSL_BN_ASM_MONT;OPENSSL_BN_ASM_MONT5;OPENSSL_BN_ASM_GF2m;SHA1_ASM;SHA256_ASM;SHA512_ASM;KECCAK1600_ASM;RC4_ASM;MD5_ASM;AESNI_ASM;VPAES_ASM;GHASH_ASM;ECP_NISTZ256_ASM;X25519_ASM;POLY1305_ASM;OPENSSLDIR=\"/etc/ssl\";ENGINESDIR=\"/dev/null\";TERMIOS;")
  set_target_properties(openssl-cli PROPERTIES COMPILE_FLAGS "-m64 -Wa,--noexecstack -Wall -O3 -pthread -m64 -Wall -O3 -Wno-missing-field-initializers -Wno-old-style-declaration -O3 -Wno-unknown-warning-option -fPIC ")
else()
  message(FATAL_ERROR "Build type must be either Debug or Release, \"${CMAKE_BUILD_TYPE}\" was specified")
endif()
set_target_properties(openssl-cli PROPERTIES LINK_FLAGS "-m64 ")
set_target_properties(openssl-cli PROPERTIES XCODE_ATTRIBUTE_GCC_VERSION "com.apple.compilers.llvm.clang.1_0")
set_target_properties(openssl-cli PROPERTIES XCODE_ATTRIBUTE_GCC_WARN_ABOUT_MISSING_NEWLINE "YES")
set_target_properties(openssl-cli PROPERTIES XCODE_ATTRIBUTE_OTHER_CFLAGS "-fstrict-aliasing -g -fPIC ")
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(openssl-cli PROPERTIES XCODE_ATTRIBUTE_GCC_OPTIMIZATION_LEVEL "0")
endif()
set_target_properties(openssl-cli PROPERTIES XCODE_ATTRIBUTE_PREBINDING "NO")
set_target_properties(openssl-cli PROPERTIES XCODE_ATTRIBUTE_WARNING_CFLAGS "-Wall -Wendif-labels -W -Wno-unused-parameter -Wundeclared-selector -Wno-parentheses-equality ")
set_target_properties(openssl-cli PROPERTIES XCODE_ATTRIBUTE_ARCHS "x86_64 ")
target_link_libraries(openssl-cli
  openssl
  "-ldl -pthread"
)

# srtp2
set(TEST_APPS OFF CACHE INTERNAL "")
set(ENABLE_OPENSSL ON CACHE INTERNAL "")
add_subdirectory(deps/libsrtp/srtp)
unset(TEST_APPS)
unset(ENABLE_OPENSSL)

#mediasoup/worker/deps/netstring/netstring.gyp:netstring#target
set(netstring__c_srcs "deps/netstring/netstring-c/netstring.c")
set(netstring__other_srcs "deps/netstring/netstring-c/netstring.h")
add_library(netstring STATIC ${netstring__c_srcs} ${netstring__other_srcs})
set_target_properties(netstring PROPERTIES EXCLUDE_FROM_ALL "FALSE")
set_source_files_properties(${netstring__other_srcs} PROPERTIES HEADER_FILE_ONLY "TRUE")
set_target_properties(netstring PROPERTIES ARCHIVE_OUTPUT_DIRECTORY "${builddir}")
set_target_properties(netstring PROPERTIES PREFIX "lib")
set_target_properties(netstring PROPERTIES ARCHIVE_OUTPUT_NAME "netstring")
set_target_properties(netstring PROPERTIES SUFFIX ".a")
set_source_files_properties(${builddir}/libnetstring.a PROPERTIES GENERATED "TRUE")
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(netstring PROPERTIES COMPILE_DEFINITIONS "DEBUG;MS_LOG_TRACE;MS_LOG_FILE_LINE;")
  set_target_properties(netstring PROPERTIES COMPILE_FLAGS "-m64 -Wall -Wno-sign-compare -g -O0 -Wno-parentheses-equality -Wno-unknown-warning-option -fPIC ")
elseif(${CMAKE_BUILD_TYPE} MATCHES "Release")
  set_target_properties(netstring PROPERTIES COMPILE_FLAGS "-m64 -Wall -Wno-sign-compare -O3 -Wno-unknown-warning-option -fPIC ")
else()
  message(FATAL_ERROR "Build type must be either Debug or Release, \"${CMAKE_BUILD_TYPE}\" was specified")
endif()
set_target_properties(netstring PROPERTIES LINK_FLAGS "-m64 ")
set_target_properties(netstring PROPERTIES XCODE_ATTRIBUTE_GCC_VERSION "com.apple.compilers.llvm.clang.1_0")
set_target_properties(netstring PROPERTIES XCODE_ATTRIBUTE_GCC_WARN_ABOUT_MISSING_NEWLINE "YES")
set_target_properties(netstring PROPERTIES XCODE_ATTRIBUTE_OTHER_CFLAGS "-fstrict-aliasing -g -fPIC ")
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(netstring PROPERTIES XCODE_ATTRIBUTE_GCC_OPTIMIZATION_LEVEL "0")
endif()
set_target_properties(netstring PROPERTIES XCODE_ATTRIBUTE_PREBINDING "NO")
set_target_properties(netstring PROPERTIES XCODE_ATTRIBUTE_WARNING_CFLAGS "-Wall -Wendif-labels -W -Wno-unused-parameter -Wundeclared-selector -Wno-parentheses-equality ")
set_target_properties(netstring PROPERTIES XCODE_ATTRIBUTE_ARCHS "x86_64 ")

#mediasoup/worker/deps/libwebrtc/deps/abseil-cpp/abseil-cpp.gyp:abseil#target
list(APPEND abseil__cxx_srcs
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/match.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/charconv_bigint.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/charconv_parse.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/str_format/arg.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/str_format/float_conversion.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/str_format/output.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/str_format/bind.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/str_format/parser.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/str_format/extension.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/memutil.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/ostringstream.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/pow10_helper.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/utf8.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/string_view.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/str_cat.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/ascii.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/numbers.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/charconv.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/str_split.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/substitute.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/escaping.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/str_replace.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/types/bad_any_cast.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/types/bad_optional_access.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/types/bad_variant_access.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/types/optional.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/blocking_counter.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/mutex.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/internal/per_thread_sem.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/internal/create_thread_identity.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/internal/waiter.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/internal/graphcycles.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/barrier.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/notification.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/hash/internal/hash.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/hash/internal/city.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/symbolize.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/failure_signal_handler.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/leak_check_disable.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/internal/examine_stack.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/internal/vdso_support.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/internal/stack_consumption.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/internal/address_is_readable.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/internal/elf_mem_image.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/internal/demangle.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/leak_check.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/stacktrace.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/src/time_zone_format.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/src/time_zone_impl.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/src/time_zone_lookup.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/src/time_zone_info.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/src/time_zone_if.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/src/time_zone_fixed.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/src/zone_info_source.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/src/time_zone_libc.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/src/civil_time_detail.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/src/time_zone_posix.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/clock.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/duration.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/civil_time.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/format.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/time.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/hashtablez_force_sampling.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/raw_hash_set.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/hashtablez_sampler_force_weak_definition.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/hashtablez_sampler.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/numeric/int128.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/spinlock_wait.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/cycleclock.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/spinlock.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/unscaledcycleclock.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/sysinfo.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/raw_logging.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/throw_delegate.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/thread_identity.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/low_level_alloc.cc"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/dynamic_annotations.cc")
list(APPEND abseil__other_srcs
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/charconv.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/numbers.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/str_format.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/str_split.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/stl_type_traits.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/char_map.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/pow10_helper.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/memutil.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/str_format/float_conversion.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/str_format/parser.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/str_format/arg.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/str_format/output.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/str_format/checker.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/str_format/extension.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/str_format/bind.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/str_join_internal.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/charconv_bigint.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/resize_uninitialized.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/ostringstream.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/charconv_parse.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/utf8.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/internal/str_split_internal.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/str_replace.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/str_join.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/ascii.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/escaping.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/match.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/strip.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/substitute.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/string_view.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/strings/str_cat.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/types/bad_variant_access.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/types/bad_any_cast.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/types/internal/variant.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/types/optional.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/types/bad_optional_access.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/types/span.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/types/any.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/types/variant.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/memory/memory.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/blocking_counter.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/notification.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/barrier.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/internal/per_thread_sem.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/internal/graphcycles.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/internal/waiter.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/internal/thread_pool.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/internal/create_thread_identity.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/internal/kernel_timeout.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/synchronization/mutex.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/hash/internal/spy_hash_state.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/hash/internal/city.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/hash/internal/hash.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/hash/hash.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/failure_signal_handler.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/stacktrace.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/symbolize.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/leak_check.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/internal/stacktrace_config.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/internal/examine_stack.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/internal/demangle.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/internal/symbolize.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/internal/address_is_readable.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/internal/stack_consumption.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/internal/elf_mem_image.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/debugging/internal/vdso_support.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/meta/type_traits.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/algorithm/algorithm.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/algorithm/container.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/time.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/include/cctz/civil_time.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/include/cctz/time_zone.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/include/cctz/zone_info_source.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/include/cctz/civil_time_detail.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/src/time_zone_fixed.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/src/time_zone_if.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/src/time_zone_posix.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/src/tzfile.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/src/time_zone_libc.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/src/time_zone_info.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/internal/cctz/src/time_zone_impl.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/civil_time.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/time/clock.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/node_hash_map.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/flat_hash_map.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/fixed_array.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/compressed_tuple.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/container_memory.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/node_hash_policy.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/raw_hash_set.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/hash_function_defaults.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/hashtablez_sampler.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/hashtable_debug.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/raw_hash_map.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/have_sse.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/layout.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/hashtable_debug_hooks.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/hash_policy_traits.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/common.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/tracked.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/internal/counting_allocator.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/inlined_vector.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/flat_hash_set.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/container/node_hash_set.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/numeric/int128.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/policy_checks.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/port.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/config.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/casts.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/spinlock.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/scheduling_mode.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/per_thread_tls.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/endian.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/sysinfo.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/tsan_mutex_interface.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/thread_identity.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/unaligned_access.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/direct_mmap.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/unscaledcycleclock.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/invoke.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/pretty_function.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/inline_variable.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/cycleclock.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/low_level_alloc.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/atomic_hook.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/identity.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/low_level_scheduling.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/raw_logging.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/throw_delegate.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/hide_ptr.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/spinlock_wait.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/internal/bits.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/attributes.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/const_init.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/macros.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/thread_annotations.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/optimization.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/log_severity.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/call_once.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/base/dynamic_annotations.h"
  "deps/libwebrtc/deps/abseil-cpp/abseil-cpp/absl/utility/utility.h")
add_library(abseil STATIC ${abseil__cxx_srcs} ${abseil__other_srcs})
set_target_properties(abseil PROPERTIES EXCLUDE_FROM_ALL "FALSE")
set_source_files_properties(${abseil__other_srcs} PROPERTIES HEADER_FILE_ONLY "TRUE")
set_target_properties(abseil PROPERTIES ARCHIVE_OUTPUT_DIRECTORY "${builddir}")
set_target_properties(abseil PROPERTIES PREFIX "lib")
set_target_properties(abseil PROPERTIES ARCHIVE_OUTPUT_NAME "abseil")
set_target_properties(abseil PROPERTIES SUFFIX ".a")
set_source_files_properties(${builddir}/libabseil.a PROPERTIES GENERATED "TRUE")
set(abseil__include_dirs "${CMAKE_CURRENT_LIST_DIR}/deps/libwebrtc/deps/abseil-cpp/abseil-cpp")
set_property(TARGET abseil APPEND PROPERTY INCLUDE_DIRECTORIES ${abseil__include_dirs})
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(abseil PROPERTIES COMPILE_DEFINITIONS "ABSL_ALLOCATOR_NOTHROW=1;DEBUG;MS_LOG_TRACE;MS_LOG_FILE_LINE;")
  set_target_properties(abseil PROPERTIES COMPILE_FLAGS "-std=c++11 -Wall -Wextra -Wno-unused-parameter -Wno-implicit-fallthrough -Wno-sign-compare -m64 -g -O0 -Wno-parentheses-equality -Wno-unknown-warning-option -fPIC ")
elseif(${CMAKE_BUILD_TYPE} MATCHES "Release")
  set_target_properties(abseil PROPERTIES COMPILE_DEFINITIONS "ABSL_ALLOCATOR_NOTHROW=1;")
  set_target_properties(abseil PROPERTIES COMPILE_FLAGS "-std=c++11 -Wall -Wextra -Wno-unused-parameter -Wno-implicit-fallthrough -Wno-sign-compare -m64 -O3 -Wno-unknown-warning-option -fPIC ")
else()
  message(FATAL_ERROR "Build type must be either Debug or Release, \"${CMAKE_BUILD_TYPE}\" was specified")
endif()
set_target_properties(abseil PROPERTIES LINK_FLAGS "-m64 ")
set_target_properties(abseil PROPERTIES XCODE_ATTRIBUTE_GCC_VERSION "com.apple.compilers.llvm.clang.1_0")
set_target_properties(abseil PROPERTIES XCODE_ATTRIBUTE_GCC_WARN_ABOUT_MISSING_NEWLINE "YES")
set_target_properties(abseil PROPERTIES XCODE_ATTRIBUTE_OTHER_CFLAGS "-fstrict-aliasing -g -fPIC ")
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(abseil PROPERTIES XCODE_ATTRIBUTE_GCC_OPTIMIZATION_LEVEL "0")
endif()
set_target_properties(abseil PROPERTIES XCODE_ATTRIBUTE_PREBINDING "NO")
set_target_properties(abseil PROPERTIES XCODE_ATTRIBUTE_WARNING_CFLAGS "-Wall -Wendif-labels -W -Wno-unused-parameter -Wundeclared-selector -Wno-parentheses-equality ")
set_target_properties(abseil PROPERTIES XCODE_ATTRIBUTE_ARCHS "x86_64 ")

# libuv
ExternalProject_Add(libuv
  URL https://dist.libuv.org/dist/v1.41.1/libuv-v1.41.1.tar.gz
  PREFIX libuv
  INSTALL_COMMAND ""
  CMAKE_ARGS -DLIBUV_BUILD_TESTS=OFF
)
ExternalProject_Get_Property (libuv BINARY_DIR)
link_directories(${BINARY_DIR})
unset(BINARY_DIR)
ExternalProject_Get_Property (libuv SOURCE_DIR)
include_directories(${SOURCE_DIR}/include)
unset(SOURCE_DIR)

#mediasoup/worker/deps/libwebrtc/libwebrtc.gyp:libwebrtc#target
list(APPEND libwebrtc__cxx_srcs
  "deps/libwebrtc/libwebrtc/system_wrappers/source/field_trial.cc"
  "deps/libwebrtc/libwebrtc/rtc_base/rate_statistics.cc"
  "deps/libwebrtc/libwebrtc/rtc_base/experiments/field_trial_parser.cc"
  "deps/libwebrtc/libwebrtc/rtc_base/experiments/alr_experiment.cc"
  "deps/libwebrtc/libwebrtc/rtc_base/experiments/field_trial_units.cc"
  "deps/libwebrtc/libwebrtc/rtc_base/experiments/rate_control_settings.cc"
  "deps/libwebrtc/libwebrtc/rtc_base/network/sent_packet.cc"
  "deps/libwebrtc/libwebrtc/call/rtp_transport_controller_send.cc"
  "deps/libwebrtc/libwebrtc/api/transport/bitrate_settings.cc"
  "deps/libwebrtc/libwebrtc/api/transport/field_trial_based_config.cc"
  "deps/libwebrtc/libwebrtc/api/transport/network_types.cc"
  "deps/libwebrtc/libwebrtc/api/transport/goog_cc_factory.cc"
  "deps/libwebrtc/libwebrtc/api/units/timestamp.cc"
  "deps/libwebrtc/libwebrtc/api/units/time_delta.cc"
  "deps/libwebrtc/libwebrtc/api/units/data_rate.cc"
  "deps/libwebrtc/libwebrtc/api/units/data_size.cc"
  "deps/libwebrtc/libwebrtc/api/units/frequency.cc"
  "deps/libwebrtc/libwebrtc/api/network_state_predictor.cc"
  "deps/libwebrtc/libwebrtc/modules/pacing/interval_budget.cc"
  "deps/libwebrtc/libwebrtc/modules/pacing/bitrate_prober.cc"
  "deps/libwebrtc/libwebrtc/modules/pacing/paced_sender.cc"
  "deps/libwebrtc/libwebrtc/modules/remote_bitrate_estimator/overuse_detector.cc"
  "deps/libwebrtc/libwebrtc/modules/remote_bitrate_estimator/overuse_estimator.cc"
  "deps/libwebrtc/libwebrtc/modules/remote_bitrate_estimator/aimd_rate_control.cc"
  "deps/libwebrtc/libwebrtc/modules/remote_bitrate_estimator/inter_arrival.cc"
  "deps/libwebrtc/libwebrtc/modules/remote_bitrate_estimator/bwe_defines.cc"
  "deps/libwebrtc/libwebrtc/modules/remote_bitrate_estimator/remote_bitrate_estimator_abs_send_time.cc"
  "deps/libwebrtc/libwebrtc/modules/rtp_rtcp/include/rtp_rtcp_defines.cc"
  "deps/libwebrtc/libwebrtc/modules/bitrate_controller/send_side_bandwidth_estimation.cc"
  "deps/libwebrtc/libwebrtc/modules/bitrate_controller/loss_based_bandwidth_estimation.cc"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/goog_cc_network_control.cc"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/probe_bitrate_estimator.cc"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/congestion_window_pushback_controller.cc"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/link_capacity_estimator.cc"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/alr_detector.cc"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/probe_controller.cc"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/median_slope_estimator.cc"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/bitrate_estimator.cc"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/trendline_estimator.cc"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/delay_based_bwe.cc"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/acknowledged_bitrate_estimator.cc"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/rtp/send_time_history.cc"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/rtp/transport_feedback_adapter.cc"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/rtp/control_handler.cc")
list(APPEND libwebrtc__other_srcs
  "deps/libwebrtc/libwebrtc/system_wrappers/source/field_trial.h"
  "deps/libwebrtc/libwebrtc/rtc_base/rate_statistics.h"
  "deps/libwebrtc/libwebrtc/rtc_base/experiments/field_trial_parser.h"
  "deps/libwebrtc/libwebrtc/rtc_base/experiments/field_trial_units.h"
  "deps/libwebrtc/libwebrtc/rtc_base/experiments/alr_experiment.h"
  "deps/libwebrtc/libwebrtc/rtc_base/experiments/rate_control_settings.h"
  "deps/libwebrtc/libwebrtc/rtc_base/network/sent_packet.h"
  "deps/libwebrtc/libwebrtc/rtc_base/units/unit_base.h"
  "deps/libwebrtc/libwebrtc/rtc_base/constructor_magic.h"
  "deps/libwebrtc/libwebrtc/rtc_base/numerics/safe_minmax.h"
  "deps/libwebrtc/libwebrtc/rtc_base/numerics/safe_conversions.h"
  "deps/libwebrtc/libwebrtc/rtc_base/numerics/safe_conversions_impl.h"
  "deps/libwebrtc/libwebrtc/rtc_base/numerics/percentile_filter.h"
  "deps/libwebrtc/libwebrtc/rtc_base/numerics/safe_compare.h"
  "deps/libwebrtc/libwebrtc/rtc_base/system/unused.h"
  "deps/libwebrtc/libwebrtc/rtc_base/type_traits.h"
  "deps/libwebrtc/libwebrtc/call/rtp_transport_controller_send.h"
  "deps/libwebrtc/libwebrtc/call/rtp_transport_controller_send_interface.h"
  "deps/libwebrtc/libwebrtc/api/transport/webrtc_key_value_config.h"
  "deps/libwebrtc/libwebrtc/api/transport/network_types.h"
  "deps/libwebrtc/libwebrtc/api/transport/bitrate_settings.h"
  "deps/libwebrtc/libwebrtc/api/transport/network_control.h"
  "deps/libwebrtc/libwebrtc/api/transport/field_trial_based_config.h"
  "deps/libwebrtc/libwebrtc/api/transport/goog_cc_factory.h"
  "deps/libwebrtc/libwebrtc/api/bitrate_constraints.h"
  "deps/libwebrtc/libwebrtc/api/units/frequency.h"
  "deps/libwebrtc/libwebrtc/api/units/data_size.h"
  "deps/libwebrtc/libwebrtc/api/units/time_delta.h"
  "deps/libwebrtc/libwebrtc/api/units/data_rate.h"
  "deps/libwebrtc/libwebrtc/api/units/timestamp.h"
  "deps/libwebrtc/libwebrtc/api/network_state_predictor.h"
  "deps/libwebrtc/libwebrtc/modules/include/module_common_types_public.h"
  "deps/libwebrtc/libwebrtc/modules/pacing/interval_budget.h"
  "deps/libwebrtc/libwebrtc/modules/pacing/paced_sender.h"
  "deps/libwebrtc/libwebrtc/modules/pacing/packet_router.h"
  "deps/libwebrtc/libwebrtc/modules/pacing/bitrate_prober.h"
  "deps/libwebrtc/libwebrtc/modules/remote_bitrate_estimator/inter_arrival.h"
  "deps/libwebrtc/libwebrtc/modules/remote_bitrate_estimator/overuse_detector.h"
  "deps/libwebrtc/libwebrtc/modules/remote_bitrate_estimator/overuse_estimator.h"
  "deps/libwebrtc/libwebrtc/modules/remote_bitrate_estimator/aimd_rate_control.h"
  "deps/libwebrtc/libwebrtc/modules/remote_bitrate_estimator/remote_bitrate_estimator_abs_send_time.h"
  "deps/libwebrtc/libwebrtc/modules/remote_bitrate_estimator/include/remote_bitrate_estimator.h"
  "deps/libwebrtc/libwebrtc/modules/rtp_rtcp/include/rtp_rtcp_defines.h"
  "deps/libwebrtc/libwebrtc/modules/rtp_rtcp/source/rtp_packet/transport_feedback.h"
  "deps/libwebrtc/libwebrtc/modules/bitrate_controller/loss_based_bandwidth_estimation.h"
  "deps/libwebrtc/libwebrtc/modules/bitrate_controller/send_side_bandwidth_estimation.h"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/bitrate_estimator.h"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/link_capacity_estimator.h"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/median_slope_estimator.h"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/probe_controller.h"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/trendline_estimator.h"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/goog_cc_network_control.h"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/delay_increase_detector_interface.h"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/acknowledged_bitrate_estimator.h"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/congestion_window_pushback_controller.h"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/delay_based_bwe.h"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/probe_bitrate_estimator.h"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/goog_cc/alr_detector.h"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/rtp/send_time_history.h"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/rtp/transport_feedback_adapter.h"
  "deps/libwebrtc/libwebrtc/modules/congestion_controller/rtp/control_handler.h"
  "deps/libwebrtc/libwebrtc/mediasoup_helpers.h")
add_library(libwebrtc STATIC ${libwebrtc__cxx_srcs} ${libwebrtc__other_srcs})
set_target_properties(libwebrtc PROPERTIES EXCLUDE_FROM_ALL "FALSE")
set_source_files_properties(${libwebrtc__other_srcs} PROPERTIES HEADER_FILE_ONLY "TRUE")
set_target_properties(libwebrtc PROPERTIES ARCHIVE_OUTPUT_DIRECTORY "${builddir}")
set_target_properties(libwebrtc PROPERTIES PREFIX "lib")
set_target_properties(libwebrtc PROPERTIES ARCHIVE_OUTPUT_NAME "webrtc")
set_target_properties(libwebrtc PROPERTIES SUFFIX ".a")
set_source_files_properties(${builddir}/libwebrtc.a PROPERTIES GENERATED "TRUE")
list(APPEND libwebrtc__include_dirs
  "${CMAKE_CURRENT_LIST_DIR}/deps/libwebrtc/libwebrtc"
  "${CMAKE_CURRENT_LIST_DIR}/include"
  "${CMAKE_CURRENT_LIST_DIR}/deps/libwebrtc/deps/abseil-cpp/abseil-cpp"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/include")
set_property(TARGET libwebrtc APPEND PROPERTY INCLUDE_DIRECTORIES ${libwebrtc__include_dirs})
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(libwebrtc PROPERTIES COMPILE_DEFINITIONS "MS_LITTLE_ENDIAN;_LARGEFILE_SOURCE;_FILE_OFFSET_BITS=64;_POSIX_C_SOURCE=200112;DEBUG;MS_LOG_TRACE;MS_LOG_FILE_LINE;")
  set_target_properties(libwebrtc PROPERTIES COMPILE_FLAGS "-std=c++11 -m64 -g -O0 -Wno-parentheses-equality -Wno-unknown-warning-option -fPIC ")
elseif(${CMAKE_BUILD_TYPE} MATCHES "Release")
  set_target_properties(libwebrtc PROPERTIES COMPILE_DEFINITIONS "MS_LITTLE_ENDIAN;_LARGEFILE_SOURCE;_FILE_OFFSET_BITS=64;_POSIX_C_SOURCE=200112;")
  set_target_properties(libwebrtc PROPERTIES COMPILE_FLAGS "-std=c++11 -m64 -O3 -Wno-unknown-warning-option -fPIC ")
else()
  message(FATAL_ERROR "Build type must be either Debug or Release, \"${CMAKE_BUILD_TYPE}\" was specified")
endif()
set_target_properties(libwebrtc PROPERTIES LINK_FLAGS "-m64 ")
set_target_properties(libwebrtc PROPERTIES XCODE_ATTRIBUTE_GCC_VERSION "com.apple.compilers.llvm.clang.1_0")
set_target_properties(libwebrtc PROPERTIES XCODE_ATTRIBUTE_GCC_WARN_ABOUT_MISSING_NEWLINE "YES")
set_target_properties(libwebrtc PROPERTIES XCODE_ATTRIBUTE_OTHER_CFLAGS "-fstrict-aliasing -g -fPIC ")
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(libwebrtc PROPERTIES XCODE_ATTRIBUTE_GCC_OPTIMIZATION_LEVEL "0")
endif()
set_target_properties(libwebrtc PROPERTIES XCODE_ATTRIBUTE_PREBINDING "NO")
set_target_properties(libwebrtc PROPERTIES XCODE_ATTRIBUTE_WARNING_CFLAGS "-Wall -Wendif-labels -W -Wno-unused-parameter -Wundeclared-selector -Wno-parentheses-equality ")
set_target_properties(libwebrtc PROPERTIES XCODE_ATTRIBUTE_ARCHS "x86_64 ")

#mediasoup/worker/mediasoup-worker.gyp:mediasoup-worker-test#target
set(mediasoup-worker-test_sources
  "test/src/tests.cpp"
  "test/src/RTC/TestKeyFrameRequestManager.cpp"
  "test/src/RTC/TestNackGenerator.cpp"
  "test/src/RTC/TestRateCalculator.cpp"
  "test/src/RTC/TestRtpPacket.cpp"
  "test/src/RTC/TestRtpStreamSend.cpp"
  "test/src/RTC/TestRtpStreamRecv.cpp"
  "test/src/RTC/TestSeqManager.cpp"
  "test/src/RTC/TestTrendCalculator.cpp"
  "test/src/RTC/TestRtpEncodingParameters.cpp"
  "test/src/RTC/Codecs/TestVP8.cpp"
  "test/src/RTC/RTCP/TestFeedbackPsAfb.cpp"
  "test/src/RTC/RTCP/TestFeedbackPsFir.cpp"
  "test/src/RTC/RTCP/TestFeedbackPsLei.cpp"
  "test/src/RTC/RTCP/TestFeedbackPsPli.cpp"
  "test/src/RTC/RTCP/TestFeedbackPsRemb.cpp"
  "test/src/RTC/RTCP/TestFeedbackPsRpsi.cpp"
  "test/src/RTC/RTCP/TestFeedbackPsSli.cpp"
  "test/src/RTC/RTCP/TestFeedbackPsTst.cpp"
  "test/src/RTC/RTCP/TestFeedbackPsVbcm.cpp"
  "test/src/RTC/RTCP/TestFeedbackRtpEcn.cpp"
  "test/src/RTC/RTCP/TestFeedbackRtpNack.cpp"
  "test/src/RTC/RTCP/TestFeedbackRtpSrReq.cpp"
  "test/src/RTC/RTCP/TestFeedbackRtpTllei.cpp"
  "test/src/RTC/RTCP/TestFeedbackRtpTmmb.cpp"
  "test/src/RTC/RTCP/TestFeedbackRtpTransport.cpp"
  "test/src/RTC/RTCP/TestBye.cpp"
  "test/src/RTC/RTCP/TestReceiverReport.cpp"
  "test/src/RTC/RTCP/TestSdes.cpp"
  "test/src/RTC/RTCP/TestSenderReport.cpp"
  "test/src/RTC/RTCP/TestPacket.cpp"
  "test/src/RTC/RTCP/TestXr.cpp"
  "test/src/Utils/TestBits.cpp"
  "test/src/Utils/TestIP.cpp"
  "test/src/Utils/TestJson.cpp"
  "test/src/Utils/TestString.cpp"
  "test/src/Utils/TestTime.cpp"
)
set(mediasoup-worker-test_headers
  "test/include/helpers.hpp"
)
add_executable(
  mediasoup-worker-test
  ${libmediasoup-worker_sources}
  ${mediasoup-worker-test_sources}
  ${libmediasoup-worker_headers}
  ${mediasoup-worker-test_headers}
)
add_dependencies(mediasoup-worker-test libuv)
list(APPEND mediasoup-worker-test__include_dirs
  "${CMAKE_CURRENT_LIST_DIR}/include"
  "${CMAKE_CURRENT_LIST_DIR}/test/include"
  "${CMAKE_CURRENT_LIST_DIR}/deps/catch/single_include/catch2"
  "${CMAKE_CURRENT_LIST_DIR}/deps/netstring/netstring-c"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/include"
  "${CMAKE_CURRENT_LIST_DIR}/deps/libwebrtc"
  "${CMAKE_CURRENT_LIST_DIR}/deps/libwebrtc/libwebrtc"
  "${CMAKE_CURRENT_LIST_DIR}/deps/libwebrtc/deps/abseil-cpp/abseil-cpp")
set_property(TARGET mediasoup-worker-test APPEND PROPERTY INCLUDE_DIRECTORIES ${mediasoup-worker-test__include_dirs})
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(mediasoup-worker-test PROPERTIES COMPILE_DEFINITIONS "MS_LITTLE_ENDIAN;_POSIX_C_SOURCE=200112;_GNU_SOURCE;MS_LOG_STD;MS_TEST;_LARGEFILE_SOURCE;_FILE_OFFSET_BITS=64;DEBUG;MS_LOG_TRACE;MS_LOG_FILE_LINE;")
  set_target_properties(mediasoup-worker-test PROPERTIES COMPILE_FLAGS "-std=c++11 -Wall -Wextra -Wno-unused-parameter -Wno-implicit-fallthrough -m64 -g -O0 -Wno-parentheses-equality -Wno-unknown-warning-option -fPIC ")
elseif(${CMAKE_BUILD_TYPE} MATCHES "Release")
  set_target_properties(mediasoup-worker-test PROPERTIES COMPILE_DEFINITIONS "MS_LITTLE_ENDIAN;_POSIX_C_SOURCE=200112;_GNU_SOURCE;MS_LOG_STD;MS_TEST;_LARGEFILE_SOURCE;_FILE_OFFSET_BITS=64;")
  set_target_properties(mediasoup-worker-test PROPERTIES COMPILE_FLAGS "-std=c++11 -Wall -Wextra -Wno-unused-parameter -Wno-implicit-fallthrough -m64 -O3 -Wno-unknown-warning-option -fPIC ")
else()
  message(FATAL_ERROR "Build type must be either Debug or Release, \"${CMAKE_BUILD_TYPE}\" was specified")
endif()
set_target_properties(mediasoup-worker-test PROPERTIES XCODE_ATTRIBUTE_GCC_VERSION "com.apple.compilers.llvm.clang.1_0")
set_target_properties(mediasoup-worker-test PROPERTIES XCODE_ATTRIBUTE_GCC_WARN_ABOUT_MISSING_NEWLINE "YES")
set_target_properties(mediasoup-worker-test PROPERTIES XCODE_ATTRIBUTE_OTHER_CFLAGS "-fstrict-aliasing -g -fPIC ")
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(mediasoup-worker-test PROPERTIES XCODE_ATTRIBUTE_GCC_OPTIMIZATION_LEVEL "0")
endif()
set_target_properties(mediasoup-worker-test PROPERTIES XCODE_ATTRIBUTE_OTHER_LDFLAGS "--coverage ")
set_target_properties(mediasoup-worker-test PROPERTIES XCODE_ATTRIBUTE_PREBINDING "NO")
set_target_properties(mediasoup-worker-test PROPERTIES XCODE_ATTRIBUTE_OTHER_CPLUSPLUSFLAGS "--coverage ")
set_target_properties(mediasoup-worker-test PROPERTIES XCODE_ATTRIBUTE_WARNING_CFLAGS "-Wall -Wendif-labels -W -Wno-unused-parameter -Wundeclared-selector -Wno-parentheses-equality ")
set_target_properties(mediasoup-worker-test PROPERTIES XCODE_ATTRIBUTE_ARCHS "x86_64 ")
target_link_libraries(mediasoup-worker-test
  openssl
  netstring
  uv_a
  srtp2
  usrsctp
  libwebrtc
  abseil
)
unset(mediasoup-worker-test_sources)
unset(mediasoup-worker-test_headers)

#mediasoup/worker/mediasoup-worker.gyp:mediasoup-worker-fuzzer#target
set(mediasoup-worker-fuzzer_sources
  "fuzzer/src/fuzzer.cpp"
  "fuzzer/src/FuzzerUtils.cpp"
  "fuzzer/src/RTC/FuzzerStunPacket.cpp"
  "fuzzer/src/RTC/FuzzerRtpPacket.cpp"
  "fuzzer/src/RTC/FuzzerTrendCalculator.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerBye.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerFeedbackPs.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerFeedbackPsAfb.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerFeedbackPsFir.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerFeedbackPsLei.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerFeedbackPsPli.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerFeedbackPsRemb.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerFeedbackPsRpsi.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerFeedbackPsSli.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerFeedbackPsTst.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerFeedbackPsVbcm.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerFeedbackRtp.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerFeedbackRtpEcn.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerFeedbackRtpNack.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerFeedbackRtpSrReq.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerFeedbackRtpTllei.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerFeedbackRtpTmmb.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerFeedbackRtpTransport.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerPacket.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerReceiverReport.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerSdes.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerSenderReport.cpp"
  "fuzzer/src/RTC/RTCP/FuzzerXr.cpp"
)
set(mediasoup-worker-fuzzer_headers
  "fuzzer/include/FuzzerUtils.hpp"
  "fuzzer/include/RTC/FuzzerRtpPacket.hpp"
  "fuzzer/include/RTC/FuzzerStunPacket.hpp"
  "fuzzer/include/RTC/FuzzerTrendCalculator.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerBye.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerFeedbackPs.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerFeedbackPsAfb.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerFeedbackPsFir.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerFeedbackPsLei.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerFeedbackPsPli.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerFeedbackPsRemb.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerFeedbackPsRpsi.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerFeedbackPsSli.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerFeedbackPsTst.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerFeedbackPsVbcm.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerFeedbackRtp.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerFeedbackRtpEcn.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerFeedbackRtpNack.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerFeedbackRtpSrReq.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerFeedbackRtpTllei.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerFeedbackRtpTmmb.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerFeedbackRtpTransport.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerPacket.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerReceiverReport.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerSdes.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerSenderReport.hpp"
  "fuzzer/include/RTC/RTCP/FuzzerXr.hpp"
)
add_executable(mediasoup-worker-fuzzer
  ${libmediasoup-worker_sources}
  ${mediasoup-worker-fuzzer_sources}
  ${libmediasoup-worker_headers}
  ${mediasoup-worker-fuzzer_headers}
)
add_dependencies(mediasoup-worker-fuzzer libuv)
list(APPEND mediasoup-worker-fuzzer__include_dirs
  "${CMAKE_CURRENT_LIST_DIR}/include"
  "${CMAKE_CURRENT_LIST_DIR}/fuzzer/include"
  "${CMAKE_CURRENT_LIST_DIR}/deps/netstring/netstring-c"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/include"
  "${CMAKE_CURRENT_LIST_DIR}/deps/libwebrtc"
  "${CMAKE_CURRENT_LIST_DIR}/deps/libwebrtc/libwebrtc"
  "${CMAKE_CURRENT_LIST_DIR}/deps/libwebrtc/deps/abseil-cpp/abseil-cpp")
set_property(TARGET mediasoup-worker-fuzzer APPEND PROPERTY INCLUDE_DIRECTORIES ${mediasoup-worker-fuzzer__include_dirs})
# TODO: -fsanitize is missing here and below
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(mediasoup-worker-fuzzer PROPERTIES COMPILE_DEFINITIONS "MS_LITTLE_ENDIAN;_POSIX_C_SOURCE=200112;_GNU_SOURCE;DEBUG;MS_LOG_STD;MS_TEST;_LARGEFILE_SOURCE;_FILE_OFFSET_BITS=64;MS_LOG_TRACE;MS_LOG_FILE_LINE;")
  set_target_properties(mediasoup-worker-fuzzer PROPERTIES COMPILE_FLAGS "-std=c++11 -Wall -Wextra -Wno-unused-parameter -Wno-implicit-fallthrough -m64 -g -O0 -g -O0 -Wno-parentheses-equality -Wno-unknown-warning-option -fPIC ")
elseif(${CMAKE_BUILD_TYPE} MATCHES "Release")
  set_target_properties(mediasoup-worker-fuzzer PROPERTIES COMPILE_DEFINITIONS "MS_LITTLE_ENDIAN;_POSIX_C_SOURCE=200112;_GNU_SOURCE;DEBUG;MS_LOG_STD;MS_TEST;_LARGEFILE_SOURCE;_FILE_OFFSET_BITS=64;")
  set_target_properties(mediasoup-worker-fuzzer PROPERTIES COMPILE_FLAGS "-std=c++11 -Wall -Wextra -Wno-unused-parameter -Wno-implicit-fallthrough -m64 -g -O0 -O3 -Wno-unknown-warning-option -fPIC ")
else()
  message(FATAL_ERROR "Build type must be either Debug or Release, \"${CMAKE_BUILD_TYPE}\" was specified")
endif()
set_target_properties(mediasoup-worker-fuzzer PROPERTIES XCODE_ATTRIBUTE_GCC_VERSION "com.apple.compilers.llvm.clang.1_0")
set_target_properties(mediasoup-worker-fuzzer PROPERTIES XCODE_ATTRIBUTE_GCC_WARN_ABOUT_MISSING_NEWLINE "YES")
set_target_properties(mediasoup-worker-fuzzer PROPERTIES XCODE_ATTRIBUTE_OTHER_CFLAGS "-fstrict-aliasing -g -fPIC ")
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(mediasoup-worker-fuzzer PROPERTIES XCODE_ATTRIBUTE_GCC_OPTIMIZATION_LEVEL "0")
endif()
set_target_properties(mediasoup-worker-fuzzer PROPERTIES XCODE_ATTRIBUTE_PREBINDING "NO")
set_target_properties(mediasoup-worker-fuzzer PROPERTIES XCODE_ATTRIBUTE_WARNING_CFLAGS "-Wall -Wendif-labels -W -Wno-unused-parameter -Wundeclared-selector -Wno-parentheses-equality ")
set_target_properties(mediasoup-worker-fuzzer PROPERTIES XCODE_ATTRIBUTE_ARCHS "x86_64 ")
target_link_libraries(mediasoup-worker-fuzzer
  openssl
  netstring
  uv_a
  srtp2
  usrsctp
  libwebrtc
  abseil
)
unset(mediasoup-worker-fuzzer_sources)
unset(mediasoup-worker-fuzzer_headers)

#mediasoup/worker/mediasoup-worker.gyp:mediasoup-worker#target
add_executable(mediasoup-worker
  "src/main.cpp"
  ${libmediasoup-worker_sources}
  ${libmediasoup-worker_headers}
)
add_dependencies(mediasoup-worker libuv)
list(APPEND mediasoup-worker__include_dirs
  "${CMAKE_CURRENT_LIST_DIR}/include"
  "${CMAKE_CURRENT_LIST_DIR}/deps/netstring/netstring-c"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/include"
  "${CMAKE_CURRENT_LIST_DIR}/deps/libwebrtc"
  "${CMAKE_CURRENT_LIST_DIR}/deps/libwebrtc/libwebrtc"
  "${CMAKE_CURRENT_LIST_DIR}/deps/libwebrtc/deps/abseil-cpp/abseil-cpp")
set_property(TARGET mediasoup-worker APPEND PROPERTY INCLUDE_DIRECTORIES ${mediasoup-worker__include_dirs})
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(mediasoup-worker PROPERTIES COMPILE_DEFINITIONS "MS_LITTLE_ENDIAN;_POSIX_C_SOURCE=200112;_GNU_SOURCE;MS_EXECUTABLE;_LARGEFILE_SOURCE;_FILE_OFFSET_BITS=64;DEBUG;MS_LOG_TRACE;MS_LOG_FILE_LINE;")
  set_target_properties(mediasoup-worker PROPERTIES COMPILE_FLAGS "-std=c++11 -Wall -Wextra -Wno-unused-parameter -Wno-implicit-fallthrough -m64 -g -O0 -Wno-parentheses-equality -Wno-unknown-warning-option -fPIC ")
elseif(${CMAKE_BUILD_TYPE} MATCHES "Release")
  set_target_properties(mediasoup-worker PROPERTIES COMPILE_DEFINITIONS "MS_LITTLE_ENDIAN;_POSIX_C_SOURCE=200112;_GNU_SOURCE;MS_EXECUTABLE;_LARGEFILE_SOURCE;_FILE_OFFSET_BITS=64;")
  set_target_properties(mediasoup-worker PROPERTIES COMPILE_FLAGS "-std=c++11 -Wall -Wextra -Wno-unused-parameter -Wno-implicit-fallthrough -m64 -O3 -Wno-unknown-warning-option -fPIC ")
else()
  message(FATAL_ERROR "Build type must be either Debug or Release, \"${CMAKE_BUILD_TYPE}\" was specified")
endif()
set_target_properties(mediasoup-worker PROPERTIES XCODE_ATTRIBUTE_GCC_VERSION "com.apple.compilers.llvm.clang.1_0")
set_target_properties(mediasoup-worker PROPERTIES XCODE_ATTRIBUTE_GCC_WARN_ABOUT_MISSING_NEWLINE "YES")
set_target_properties(mediasoup-worker PROPERTIES XCODE_ATTRIBUTE_OTHER_CFLAGS "-fstrict-aliasing -g -fPIC ")
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(mediasoup-worker PROPERTIES XCODE_ATTRIBUTE_GCC_OPTIMIZATION_LEVEL "0")
endif()
set_target_properties(mediasoup-worker PROPERTIES XCODE_ATTRIBUTE_PREBINDING "NO")
set_target_properties(mediasoup-worker PROPERTIES XCODE_ATTRIBUTE_WARNING_CFLAGS "-Wall -Wendif-labels -W -Wno-unused-parameter -Wundeclared-selector -Wno-parentheses-equality ")
set_target_properties(mediasoup-worker PROPERTIES XCODE_ATTRIBUTE_ARCHS "x86_64 ")
target_link_libraries(mediasoup-worker
  openssl
  netstring
  uv_a
  srtp2
  usrsctp
  libwebrtc
  abseil
)

#mediasoup/worker/mediasoup-worker.gyp:libmediasoup-worker#target
add_library(libmediasoup-worker
  STATIC ${libmediasoup-worker_sources} ${libmediasoup-worker_haaders}
)
add_dependencies(libmediasoup-worker libuv)
set_target_properties(libmediasoup-worker PROPERTIES EXCLUDE_FROM_ALL "FALSE")
set_target_properties(libmediasoup-worker PROPERTIES PREFIX "lib")
list(APPEND libmediasoup-worker__include_dirs
  "${CMAKE_CURRENT_LIST_DIR}/include"
  "${CMAKE_CURRENT_LIST_DIR}/deps/netstring/netstring-c"
  "${CMAKE_CURRENT_LIST_DIR}/deps/openssl/openssl/include"
  "${CMAKE_CURRENT_LIST_DIR}/deps/libwebrtc"
  "${CMAKE_CURRENT_LIST_DIR}/deps/libwebrtc/libwebrtc"
  "${CMAKE_CURRENT_LIST_DIR}/deps/libwebrtc/deps/abseil-cpp/abseil-cpp")
set_property(TARGET libmediasoup-worker APPEND PROPERTY INCLUDE_DIRECTORIES ${libmediasoup-worker__include_dirs})
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(libmediasoup-worker PROPERTIES COMPILE_DEFINITIONS "MS_LITTLE_ENDIAN;_POSIX_C_SOURCE=200112;_GNU_SOURCE;_LARGEFILE_SOURCE;_FILE_OFFSET_BITS=64;DEBUG;MS_LOG_TRACE;MS_LOG_FILE_LINE;")
  set_target_properties(libmediasoup-worker PROPERTIES COMPILE_FLAGS "-std=c++11 -Wall -Wextra -Wno-unused-parameter -Wno-implicit-fallthrough -m64 -g -O0 -Wno-parentheses-equality -Wno-unknown-warning-option -fPIC ")
elseif(${CMAKE_BUILD_TYPE} MATCHES "Release")
  set_target_properties(libmediasoup-worker PROPERTIES COMPILE_DEFINITIONS "MS_LITTLE_ENDIAN;_POSIX_C_SOURCE=200112;_GNU_SOURCE;_LARGEFILE_SOURCE;_FILE_OFFSET_BITS=64;")
  set_target_properties(libmediasoup-worker PROPERTIES COMPILE_FLAGS "-std=c++11 -Wall -Wextra -Wno-unused-parameter -Wno-implicit-fallthrough -m64 -O3 -Wno-unknown-warning-option -fPIC ")
else()
  message(FATAL_ERROR "Build type must be either Debug or Release, \"${CMAKE_BUILD_TYPE}\" was specified")
endif()
set_target_properties(libmediasoup-worker PROPERTIES XCODE_ATTRIBUTE_GCC_VERSION "com.apple.compilers.llvm.clang.1_0")
set_target_properties(libmediasoup-worker PROPERTIES XCODE_ATTRIBUTE_GCC_WARN_ABOUT_MISSING_NEWLINE "YES")
set_target_properties(libmediasoup-worker PROPERTIES XCODE_ATTRIBUTE_OTHER_CFLAGS "-fstrict-aliasing -g -fPIC ")
if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  set_target_properties(libmediasoup-worker PROPERTIES XCODE_ATTRIBUTE_GCC_OPTIMIZATION_LEVEL "0")
endif()
set_target_properties(libmediasoup-worker PROPERTIES XCODE_ATTRIBUTE_PREBINDING "NO")
set_target_properties(libmediasoup-worker PROPERTIES XCODE_ATTRIBUTE_WARNING_CFLAGS "-Wall -Wendif-labels -W -Wno-unused-parameter -Wundeclared-selector -Wno-parentheses-equality ")
set_target_properties(libmediasoup-worker PROPERTIES XCODE_ATTRIBUTE_ARCHS "x86_64 ")
target_link_libraries(libmediasoup-worker
  openssl
  netstring
  uv_a
  srtp2
  usrsctp
  libwebrtc
  abseil
)
