<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

Part VIII. Appendixes

</div>

[Prev](planner-stats-security.html "68.3. Planner Statistics and Security") 

 

 

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](errcodes-appendix.html "Appendix A. PostgreSQL Error Codes")

-----

<div id="APPENDIXES" class="part">

<div class="titlepage">

<div>

<div>

</div>

</div>

</div>

<div class="toc">

**Table of Contents**

<span class="appendix">[A. <span class="productname">PostgreSQL</span>
Error Codes](errcodes-appendix.html)</span>

<span class="appendix">[B. Date/Time
Support](datetime-appendix.html)</span>

<span class="sect1">[B.1. Date/Time Input
Interpretation](datetime-input-rules.html)</span>

<span class="sect1">[B.2. Date/Time Key
Words](datetime-keywords.html)</span>

<span class="sect1">[B.3. Date/Time Configuration
Files](datetime-config-files.html)</span>

<span class="sect1">[B.4. History of
Units](datetime-units-history.html)</span>

<span class="appendix">[C. SQL Key
Words](sql-keywords-appendix.html)</span>

<span class="appendix">[D. SQL Conformance](features.html)</span>

<span class="sect1">[D.1. Supported
Features](features-sql-standard.html)</span>

<span class="sect1">[D.2. Unsupported
Features](unsupported-features-sql-standard.html)</span>

<span class="appendix">[E. Release Notes](release.html)</span>

<span class="sect1">[E.1. Release 10.3](release-10-3.html)</span>

<span class="sect1">[E.2. Release 10.2](release-10-2.html)</span>

<span class="sect1">[E.3. Release 10.1](release-10-1.html)</span>

<span class="sect1">[E.4. Release 10](release-10.html)</span>

<span class="sect1">[E.5. Release 9.6.8](release-9-6-8.html)</span>

<span class="sect1">[E.6. Release 9.6.7](release-9-6-7.html)</span>

<span class="sect1">[E.7. Release 9.6.6](release-9-6-6.html)</span>

<span class="sect1">[E.8. Release 9.6.5](release-9-6-5.html)</span>

<span class="sect1">[E.9. Release 9.6.4](release-9-6-4.html)</span>

<span class="sect1">[E.10. Release 9.6.3](release-9-6-3.html)</span>

<span class="sect1">[E.11. Release 9.6.2](release-9-6-2.html)</span>

<span class="sect1">[E.12. Release 9.6.1](release-9-6-1.html)</span>

<span class="sect1">[E.13. Release 9.6](release-9-6.html)</span>

<span class="sect1">[E.14. Release 9.5.12](release-9-5-12.html)</span>

<span class="sect1">[E.15. Release 9.5.11](release-9-5-11.html)</span>

<span class="sect1">[E.16. Release 9.5.10](release-9-5-10.html)</span>

<span class="sect1">[E.17. Release 9.5.9](release-9-5-9.html)</span>

<span class="sect1">[E.18. Release 9.5.8](release-9-5-8.html)</span>

<span class="sect1">[E.19. Release 9.5.7](release-9-5-7.html)</span>

<span class="sect1">[E.20. Release 9.5.6](release-9-5-6.html)</span>

<span class="sect1">[E.21. Release 9.5.5](release-9-5-5.html)</span>

<span class="sect1">[E.22. Release 9.5.4](release-9-5-4.html)</span>

<span class="sect1">[E.23. Release 9.5.3](release-9-5-3.html)</span>

<span class="sect1">[E.24. Release 9.5.2](release-9-5-2.html)</span>

<span class="sect1">[E.25. Release 9.5.1](release-9-5-1.html)</span>

<span class="sect1">[E.26. Release 9.5](release-9-5.html)</span>

<span class="sect1">[E.27. Release 9.4.17](release-9-4-17.html)</span>

<span class="sect1">[E.28. Release 9.4.16](release-9-4-16.html)</span>

<span class="sect1">[E.29. Release 9.4.15](release-9-4-15.html)</span>

<span class="sect1">[E.30. Release 9.4.14](release-9-4-14.html)</span>

<span class="sect1">[E.31. Release 9.4.13](release-9-4-13.html)</span>

<span class="sect1">[E.32. Release 9.4.12](release-9-4-12.html)</span>

<span class="sect1">[E.33. Release 9.4.11](release-9-4-11.html)</span>

<span class="sect1">[E.34. Release 9.4.10](release-9-4-10.html)</span>

<span class="sect1">[E.35. Release 9.4.9](release-9-4-9.html)</span>

<span class="sect1">[E.36. Release 9.4.8](release-9-4-8.html)</span>

<span class="sect1">[E.37. Release 9.4.7](release-9-4-7.html)</span>

<span class="sect1">[E.38. Release 9.4.6](release-9-4-6.html)</span>

<span class="sect1">[E.39. Release 9.4.5](release-9-4-5.html)</span>

<span class="sect1">[E.40. Release 9.4.4](release-9-4-4.html)</span>

<span class="sect1">[E.41. Release 9.4.3](release-9-4-3.html)</span>

<span class="sect1">[E.42. Release 9.4.2](release-9-4-2.html)</span>

<span class="sect1">[E.43. Release 9.4.1](release-9-4-1.html)</span>

<span class="sect1">[E.44. Release 9.4](release-9-4.html)</span>

<span class="sect1">[E.45. Release 9.3.22](release-9-3-22.html)</span>

<span class="sect1">[E.46. Release 9.3.21](release-9-3-21.html)</span>

<span class="sect1">[E.47. Release 9.3.20](release-9-3-20.html)</span>

<span class="sect1">[E.48. Release 9.3.19](release-9-3-19.html)</span>

<span class="sect1">[E.49. Release 9.3.18](release-9-3-18.html)</span>

<span class="sect1">[E.50. Release 9.3.17](release-9-3-17.html)</span>

<span class="sect1">[E.51. Release 9.3.16](release-9-3-16.html)</span>

<span class="sect1">[E.52. Release 9.3.15](release-9-3-15.html)</span>

<span class="sect1">[E.53. Release 9.3.14](release-9-3-14.html)</span>

<span class="sect1">[E.54. Release 9.3.13](release-9-3-13.html)</span>

<span class="sect1">[E.55. Release 9.3.12](release-9-3-12.html)</span>

<span class="sect1">[E.56. Release 9.3.11](release-9-3-11.html)</span>

<span class="sect1">[E.57. Release 9.3.10](release-9-3-10.html)</span>

<span class="sect1">[E.58. Release 9.3.9](release-9-3-9.html)</span>

<span class="sect1">[E.59. Release 9.3.8](release-9-3-8.html)</span>

<span class="sect1">[E.60. Release 9.3.7](release-9-3-7.html)</span>

<span class="sect1">[E.61. Release 9.3.6](release-9-3-6.html)</span>

<span class="sect1">[E.62. Release 9.3.5](release-9-3-5.html)</span>

<span class="sect1">[E.63. Release 9.3.4](release-9-3-4.html)</span>

<span class="sect1">[E.64. Release 9.3.3](release-9-3-3.html)</span>

<span class="sect1">[E.65. Release 9.3.2](release-9-3-2.html)</span>

<span class="sect1">[E.66. Release 9.3.1](release-9-3-1.html)</span>

<span class="sect1">[E.67. Release 9.3](release-9-3.html)</span>

<span class="sect1">[E.68. Release 9.2.24](release-9-2-24.html)</span>

<span class="sect1">[E.69. Release 9.2.23](release-9-2-23.html)</span>

<span class="sect1">[E.70. Release 9.2.22](release-9-2-22.html)</span>

<span class="sect1">[E.71. Release 9.2.21](release-9-2-21.html)</span>

<span class="sect1">[E.72. Release 9.2.20](release-9-2-20.html)</span>

<span class="sect1">[E.73. Release 9.2.19](release-9-2-19.html)</span>

<span class="sect1">[E.74. Release 9.2.18](release-9-2-18.html)</span>

<span class="sect1">[E.75. Release 9.2.17](release-9-2-17.html)</span>

<span class="sect1">[E.76. Release 9.2.16](release-9-2-16.html)</span>

<span class="sect1">[E.77. Release 9.2.15](release-9-2-15.html)</span>

<span class="sect1">[E.78. Release 9.2.14](release-9-2-14.html)</span>

<span class="sect1">[E.79. Release 9.2.13](release-9-2-13.html)</span>

<span class="sect1">[E.80. Release 9.2.12](release-9-2-12.html)</span>

<span class="sect1">[E.81. Release 9.2.11](release-9-2-11.html)</span>

<span class="sect1">[E.82. Release 9.2.10](release-9-2-10.html)</span>

<span class="sect1">[E.83. Release 9.2.9](release-9-2-9.html)</span>

<span class="sect1">[E.84. Release 9.2.8](release-9-2-8.html)</span>

<span class="sect1">[E.85. Release 9.2.7](release-9-2-7.html)</span>

<span class="sect1">[E.86. Release 9.2.6](release-9-2-6.html)</span>

<span class="sect1">[E.87. Release 9.2.5](release-9-2-5.html)</span>

<span class="sect1">[E.88. Release 9.2.4](release-9-2-4.html)</span>

<span class="sect1">[E.89. Release 9.2.3](release-9-2-3.html)</span>

<span class="sect1">[E.90. Release 9.2.2](release-9-2-2.html)</span>

<span class="sect1">[E.91. Release 9.2.1](release-9-2-1.html)</span>

<span class="sect1">[E.92. Release 9.2](release-9-2.html)</span>

<span class="sect1">[E.93. Release 9.1.24](release-9-1-24.html)</span>

<span class="sect1">[E.94. Release 9.1.23](release-9-1-23.html)</span>

<span class="sect1">[E.95. Release 9.1.22](release-9-1-22.html)</span>

<span class="sect1">[E.96. Release 9.1.21](release-9-1-21.html)</span>

<span class="sect1">[E.97. Release 9.1.20](release-9-1-20.html)</span>

<span class="sect1">[E.98. Release 9.1.19](release-9-1-19.html)</span>

<span class="sect1">[E.99. Release 9.1.18](release-9-1-18.html)</span>

<span class="sect1">[E.100. Release 9.1.17](release-9-1-17.html)</span>

<span class="sect1">[E.101. Release 9.1.16](release-9-1-16.html)</span>

<span class="sect1">[E.102. Release 9.1.15](release-9-1-15.html)</span>

<span class="sect1">[E.103. Release 9.1.14](release-9-1-14.html)</span>

<span class="sect1">[E.104. Release 9.1.13](release-9-1-13.html)</span>

<span class="sect1">[E.105. Release 9.1.12](release-9-1-12.html)</span>

<span class="sect1">[E.106. Release 9.1.11](release-9-1-11.html)</span>

<span class="sect1">[E.107. Release 9.1.10](release-9-1-10.html)</span>

<span class="sect1">[E.108. Release 9.1.9](release-9-1-9.html)</span>

<span class="sect1">[E.109. Release 9.1.8](release-9-1-8.html)</span>

<span class="sect1">[E.110. Release 9.1.7](release-9-1-7.html)</span>

<span class="sect1">[E.111. Release 9.1.6](release-9-1-6.html)</span>

<span class="sect1">[E.112. Release 9.1.5](release-9-1-5.html)</span>

<span class="sect1">[E.113. Release 9.1.4](release-9-1-4.html)</span>

<span class="sect1">[E.114. Release 9.1.3](release-9-1-3.html)</span>

<span class="sect1">[E.115. Release 9.1.2](release-9-1-2.html)</span>

<span class="sect1">[E.116. Release 9.1.1](release-9-1-1.html)</span>

<span class="sect1">[E.117. Release 9.1](release-9-1.html)</span>

<span class="sect1">[E.118. Release 9.0.23](release-9-0-23.html)</span>

<span class="sect1">[E.119. Release 9.0.22](release-9-0-22.html)</span>

<span class="sect1">[E.120. Release 9.0.21](release-9-0-21.html)</span>

<span class="sect1">[E.121. Release 9.0.20](release-9-0-20.html)</span>

<span class="sect1">[E.122. Release 9.0.19](release-9-0-19.html)</span>

<span class="sect1">[E.123. Release 9.0.18](release-9-0-18.html)</span>

<span class="sect1">[E.124. Release 9.0.17](release-9-0-17.html)</span>

<span class="sect1">[E.125. Release 9.0.16](release-9-0-16.html)</span>

<span class="sect1">[E.126. Release 9.0.15](release-9-0-15.html)</span>

<span class="sect1">[E.127. Release 9.0.14](release-9-0-14.html)</span>

<span class="sect1">[E.128. Release 9.0.13](release-9-0-13.html)</span>

<span class="sect1">[E.129. Release 9.0.12](release-9-0-12.html)</span>

<span class="sect1">[E.130. Release 9.0.11](release-9-0-11.html)</span>

<span class="sect1">[E.131. Release 9.0.10](release-9-0-10.html)</span>

<span class="sect1">[E.132. Release 9.0.9](release-9-0-9.html)</span>

<span class="sect1">[E.133. Release 9.0.8](release-9-0-8.html)</span>

<span class="sect1">[E.134. Release 9.0.7](release-9-0-7.html)</span>

<span class="sect1">[E.135. Release 9.0.6](release-9-0-6.html)</span>

<span class="sect1">[E.136. Release 9.0.5](release-9-0-5.html)</span>

<span class="sect1">[E.137. Release 9.0.4](release-9-0-4.html)</span>

<span class="sect1">[E.138. Release 9.0.3](release-9-0-3.html)</span>

<span class="sect1">[E.139. Release 9.0.2](release-9-0-2.html)</span>

<span class="sect1">[E.140. Release 9.0.1](release-9-0-1.html)</span>

<span class="sect1">[E.141. Release 9.0](release-9-0.html)</span>

<span class="sect1">[E.142. Release 8.4.22](release-8-4-22.html)</span>

<span class="sect1">[E.143. Release 8.4.21](release-8-4-21.html)</span>

<span class="sect1">[E.144. Release 8.4.20](release-8-4-20.html)</span>

<span class="sect1">[E.145. Release 8.4.19](release-8-4-19.html)</span>

<span class="sect1">[E.146. Release 8.4.18](release-8-4-18.html)</span>

<span class="sect1">[E.147. Release 8.4.17](release-8-4-17.html)</span>

<span class="sect1">[E.148. Release 8.4.16](release-8-4-16.html)</span>

<span class="sect1">[E.149. Release 8.4.15](release-8-4-15.html)</span>

<span class="sect1">[E.150. Release 8.4.14](release-8-4-14.html)</span>

<span class="sect1">[E.151. Release 8.4.13](release-8-4-13.html)</span>

<span class="sect1">[E.152. Release 8.4.12](release-8-4-12.html)</span>

<span class="sect1">[E.153. Release 8.4.11](release-8-4-11.html)</span>

<span class="sect1">[E.154. Release 8.4.10](release-8-4-10.html)</span>

<span class="sect1">[E.155. Release 8.4.9](release-8-4-9.html)</span>

<span class="sect1">[E.156. Release 8.4.8](release-8-4-8.html)</span>

<span class="sect1">[E.157. Release 8.4.7](release-8-4-7.html)</span>

<span class="sect1">[E.158. Release 8.4.6](release-8-4-6.html)</span>

<span class="sect1">[E.159. Release 8.4.5](release-8-4-5.html)</span>

<span class="sect1">[E.160. Release 8.4.4](release-8-4-4.html)</span>

<span class="sect1">[E.161. Release 8.4.3](release-8-4-3.html)</span>

<span class="sect1">[E.162. Release 8.4.2](release-8-4-2.html)</span>

<span class="sect1">[E.163. Release 8.4.1](release-8-4-1.html)</span>

<span class="sect1">[E.164. Release 8.4](release-8-4.html)</span>

<span class="sect1">[E.165. Release 8.3.23](release-8-3-23.html)</span>

<span class="sect1">[E.166. Release 8.3.22](release-8-3-22.html)</span>

<span class="sect1">[E.167. Release 8.3.21](release-8-3-21.html)</span>

<span class="sect1">[E.168. Release 8.3.20](release-8-3-20.html)</span>

<span class="sect1">[E.169. Release 8.3.19](release-8-3-19.html)</span>

<span class="sect1">[E.170. Release 8.3.18](release-8-3-18.html)</span>

<span class="sect1">[E.171. Release 8.3.17](release-8-3-17.html)</span>

<span class="sect1">[E.172. Release 8.3.16](release-8-3-16.html)</span>

<span class="sect1">[E.173. Release 8.3.15](release-8-3-15.html)</span>

<span class="sect1">[E.174. Release 8.3.14](release-8-3-14.html)</span>

<span class="sect1">[E.175. Release 8.3.13](release-8-3-13.html)</span>

<span class="sect1">[E.176. Release 8.3.12](release-8-3-12.html)</span>

<span class="sect1">[E.177. Release 8.3.11](release-8-3-11.html)</span>

<span class="sect1">[E.178. Release 8.3.10](release-8-3-10.html)</span>

<span class="sect1">[E.179. Release 8.3.9](release-8-3-9.html)</span>

<span class="sect1">[E.180. Release 8.3.8](release-8-3-8.html)</span>

<span class="sect1">[E.181. Release 8.3.7](release-8-3-7.html)</span>

<span class="sect1">[E.182. Release 8.3.6](release-8-3-6.html)</span>

<span class="sect1">[E.183. Release 8.3.5](release-8-3-5.html)</span>

<span class="sect1">[E.184. Release 8.3.4](release-8-3-4.html)</span>

<span class="sect1">[E.185. Release 8.3.3](release-8-3-3.html)</span>

<span class="sect1">[E.186. Release 8.3.2](release-8-3-2.html)</span>

<span class="sect1">[E.187. Release 8.3.1](release-8-3-1.html)</span>

<span class="sect1">[E.188. Release 8.3](release-8-3.html)</span>

<span class="sect1">[E.189. Release 8.2.23](release-8-2-23.html)</span>

<span class="sect1">[E.190. Release 8.2.22](release-8-2-22.html)</span>

<span class="sect1">[E.191. Release 8.2.21](release-8-2-21.html)</span>

<span class="sect1">[E.192. Release 8.2.20](release-8-2-20.html)</span>

<span class="sect1">[E.193. Release 8.2.19](release-8-2-19.html)</span>

<span class="sect1">[E.194. Release 8.2.18](release-8-2-18.html)</span>

<span class="sect1">[E.195. Release 8.2.17](release-8-2-17.html)</span>

<span class="sect1">[E.196. Release 8.2.16](release-8-2-16.html)</span>

<span class="sect1">[E.197. Release 8.2.15](release-8-2-15.html)</span>

<span class="sect1">[E.198. Release 8.2.14](release-8-2-14.html)</span>

<span class="sect1">[E.199. Release 8.2.13](release-8-2-13.html)</span>

<span class="sect1">[E.200. Release 8.2.12](release-8-2-12.html)</span>

<span class="sect1">[E.201. Release 8.2.11](release-8-2-11.html)</span>

<span class="sect1">[E.202. Release 8.2.10](release-8-2-10.html)</span>

<span class="sect1">[E.203. Release 8.2.9](release-8-2-9.html)</span>

<span class="sect1">[E.204. Release 8.2.8](release-8-2-8.html)</span>

<span class="sect1">[E.205. Release 8.2.7](release-8-2-7.html)</span>

<span class="sect1">[E.206. Release 8.2.6](release-8-2-6.html)</span>

<span class="sect1">[E.207. Release 8.2.5](release-8-2-5.html)</span>

<span class="sect1">[E.208. Release 8.2.4](release-8-2-4.html)</span>

<span class="sect1">[E.209. Release 8.2.3](release-8-2-3.html)</span>

<span class="sect1">[E.210. Release 8.2.2](release-8-2-2.html)</span>

<span class="sect1">[E.211. Release 8.2.1](release-8-2-1.html)</span>

<span class="sect1">[E.212. Release 8.2](release-8-2.html)</span>

<span class="sect1">[E.213. Release 8.1.23](release-8-1-23.html)</span>

<span class="sect1">[E.214. Release 8.1.22](release-8-1-22.html)</span>

<span class="sect1">[E.215. Release 8.1.21](release-8-1-21.html)</span>

<span class="sect1">[E.216. Release 8.1.20](release-8-1-20.html)</span>

<span class="sect1">[E.217. Release 8.1.19](release-8-1-19.html)</span>

<span class="sect1">[E.218. Release 8.1.18](release-8-1-18.html)</span>

<span class="sect1">[E.219. Release 8.1.17](release-8-1-17.html)</span>

<span class="sect1">[E.220. Release 8.1.16](release-8-1-16.html)</span>

<span class="sect1">[E.221. Release 8.1.15](release-8-1-15.html)</span>

<span class="sect1">[E.222. Release 8.1.14](release-8-1-14.html)</span>

<span class="sect1">[E.223. Release 8.1.13](release-8-1-13.html)</span>

<span class="sect1">[E.224. Release 8.1.12](release-8-1-12.html)</span>

<span class="sect1">[E.225. Release 8.1.11](release-8-1-11.html)</span>

<span class="sect1">[E.226. Release 8.1.10](release-8-1-10.html)</span>

<span class="sect1">[E.227. Release 8.1.9](release-8-1-9.html)</span>

<span class="sect1">[E.228. Release 8.1.8](release-8-1-8.html)</span>

<span class="sect1">[E.229. Release 8.1.7](release-8-1-7.html)</span>

<span class="sect1">[E.230. Release 8.1.6](release-8-1-6.html)</span>

<span class="sect1">[E.231. Release 8.1.5](release-8-1-5.html)</span>

<span class="sect1">[E.232. Release 8.1.4](release-8-1-4.html)</span>

<span class="sect1">[E.233. Release 8.1.3](release-8-1-3.html)</span>

<span class="sect1">[E.234. Release 8.1.2](release-8-1-2.html)</span>

<span class="sect1">[E.235. Release 8.1.1](release-8-1-1.html)</span>

<span class="sect1">[E.236. Release 8.1](release-8-1.html)</span>

<span class="sect1">[E.237. Release 8.0.26](release-8-0-26.html)</span>

<span class="sect1">[E.238. Release 8.0.25](release-8-0-25.html)</span>

<span class="sect1">[E.239. Release 8.0.24](release-8-0-24.html)</span>

<span class="sect1">[E.240. Release 8.0.23](release-8-0-23.html)</span>

<span class="sect1">[E.241. Release 8.0.22](release-8-0-22.html)</span>

<span class="sect1">[E.242. Release 8.0.21](release-8-0-21.html)</span>

<span class="sect1">[E.243. Release 8.0.20](release-8-0-20.html)</span>

<span class="sect1">[E.244. Release 8.0.19](release-8-0-19.html)</span>

<span class="sect1">[E.245. Release 8.0.18](release-8-0-18.html)</span>

<span class="sect1">[E.246. Release 8.0.17](release-8-0-17.html)</span>

<span class="sect1">[E.247. Release 8.0.16](release-8-0-16.html)</span>

<span class="sect1">[E.248. Release 8.0.15](release-8-0-15.html)</span>

<span class="sect1">[E.249. Release 8.0.14](release-8-0-14.html)</span>

<span class="sect1">[E.250. Release 8.0.13](release-8-0-13.html)</span>

<span class="sect1">[E.251. Release 8.0.12](release-8-0-12.html)</span>

<span class="sect1">[E.252. Release 8.0.11](release-8-0-11.html)</span>

<span class="sect1">[E.253. Release 8.0.10](release-8-0-10.html)</span>

<span class="sect1">[E.254. Release 8.0.9](release-8-0-9.html)</span>

<span class="sect1">[E.255. Release 8.0.8](release-8-0-8.html)</span>

<span class="sect1">[E.256. Release 8.0.7](release-8-0-7.html)</span>

<span class="sect1">[E.257. Release 8.0.6](release-8-0-6.html)</span>

<span class="sect1">[E.258. Release 8.0.5](release-8-0-5.html)</span>

<span class="sect1">[E.259. Release 8.0.4](release-8-0-4.html)</span>

<span class="sect1">[E.260. Release 8.0.3](release-8-0-3.html)</span>

<span class="sect1">[E.261. Release 8.0.2](release-8-0-2.html)</span>

<span class="sect1">[E.262. Release 8.0.1](release-8-0-1.html)</span>

<span class="sect1">[E.263. Release 8.0](release-8-0.html)</span>

<span class="sect1">[E.264. Release 7.4.30](release-7-4-30.html)</span>

<span class="sect1">[E.265. Release 7.4.29](release-7-4-29.html)</span>

<span class="sect1">[E.266. Release 7.4.28](release-7-4-28.html)</span>

<span class="sect1">[E.267. Release 7.4.27](release-7-4-27.html)</span>

<span class="sect1">[E.268. Release 7.4.26](release-7-4-26.html)</span>

<span class="sect1">[E.269. Release 7.4.25](release-7-4-25.html)</span>

<span class="sect1">[E.270. Release 7.4.24](release-7-4-24.html)</span>

<span class="sect1">[E.271. Release 7.4.23](release-7-4-23.html)</span>

<span class="sect1">[E.272. Release 7.4.22](release-7-4-22.html)</span>

<span class="sect1">[E.273. Release 7.4.21](release-7-4-21.html)</span>

<span class="sect1">[E.274. Release 7.4.20](release-7-4-20.html)</span>

<span class="sect1">[E.275. Release 7.4.19](release-7-4-19.html)</span>

<span class="sect1">[E.276. Release 7.4.18](release-7-4-18.html)</span>

<span class="sect1">[E.277. Release 7.4.17](release-7-4-17.html)</span>

<span class="sect1">[E.278. Release 7.4.16](release-7-4-16.html)</span>

<span class="sect1">[E.279. Release 7.4.15](release-7-4-15.html)</span>

<span class="sect1">[E.280. Release 7.4.14](release-7-4-14.html)</span>

<span class="sect1">[E.281. Release 7.4.13](release-7-4-13.html)</span>

<span class="sect1">[E.282. Release 7.4.12](release-7-4-12.html)</span>

<span class="sect1">[E.283. Release 7.4.11](release-7-4-11.html)</span>

<span class="sect1">[E.284. Release 7.4.10](release-7-4-10.html)</span>

<span class="sect1">[E.285. Release 7.4.9](release-7-4-9.html)</span>

<span class="sect1">[E.286. Release 7.4.8](release-7-4-8.html)</span>

<span class="sect1">[E.287. Release 7.4.7](release-7-4-7.html)</span>

<span class="sect1">[E.288. Release 7.4.6](release-7-4-6.html)</span>

<span class="sect1">[E.289. Release 7.4.5](release-7-4-5.html)</span>

<span class="sect1">[E.290. Release 7.4.4](release-7-4-4.html)</span>

<span class="sect1">[E.291. Release 7.4.3](release-7-4-3.html)</span>

<span class="sect1">[E.292. Release 7.4.2](release-7-4-2.html)</span>

<span class="sect1">[E.293. Release 7.4.1](release-7-4-1.html)</span>

<span class="sect1">[E.294. Release 7.4](release-7-4.html)</span>

<span class="sect1">[E.295. Release 7.3.21](release-7-3-21.html)</span>

<span class="sect1">[E.296. Release 7.3.20](release-7-3-20.html)</span>

<span class="sect1">[E.297. Release 7.3.19](release-7-3-19.html)</span>

<span class="sect1">[E.298. Release 7.3.18](release-7-3-18.html)</span>

<span class="sect1">[E.299. Release 7.3.17](release-7-3-17.html)</span>

<span class="sect1">[E.300. Release 7.3.16](release-7-3-16.html)</span>

<span class="sect1">[E.301. Release 7.3.15](release-7-3-15.html)</span>

<span class="sect1">[E.302. Release 7.3.14](release-7-3-14.html)</span>

<span class="sect1">[E.303. Release 7.3.13](release-7-3-13.html)</span>

<span class="sect1">[E.304. Release 7.3.12](release-7-3-12.html)</span>

<span class="sect1">[E.305. Release 7.3.11](release-7-3-11.html)</span>

<span class="sect1">[E.306. Release 7.3.10](release-7-3-10.html)</span>

<span class="sect1">[E.307. Release 7.3.9](release-7-3-9.html)</span>

<span class="sect1">[E.308. Release 7.3.8](release-7-3-8.html)</span>

<span class="sect1">[E.309. Release 7.3.7](release-7-3-7.html)</span>

<span class="sect1">[E.310. Release 7.3.6](release-7-3-6.html)</span>

<span class="sect1">[E.311. Release 7.3.5](release-7-3-5.html)</span>

<span class="sect1">[E.312. Release 7.3.4](release-7-3-4.html)</span>

<span class="sect1">[E.313. Release 7.3.3](release-7-3-3.html)</span>

<span class="sect1">[E.314. Release 7.3.2](release-7-3-2.html)</span>

<span class="sect1">[E.315. Release 7.3.1](release-7-3-1.html)</span>

<span class="sect1">[E.316. Release 7.3](release-7-3.html)</span>

<span class="sect1">[E.317. Release 7.2.8](release-7-2-8.html)</span>

<span class="sect1">[E.318. Release 7.2.7](release-7-2-7.html)</span>

<span class="sect1">[E.319. Release 7.2.6](release-7-2-6.html)</span>

<span class="sect1">[E.320. Release 7.2.5](release-7-2-5.html)</span>

<span class="sect1">[E.321. Release 7.2.4](release-7-2-4.html)</span>

<span class="sect1">[E.322. Release 7.2.3](release-7-2-3.html)</span>

<span class="sect1">[E.323. Release 7.2.2](release-7-2-2.html)</span>

<span class="sect1">[E.324. Release 7.2.1](release-7-2-1.html)</span>

<span class="sect1">[E.325. Release 7.2](release-7-2.html)</span>

<span class="sect1">[E.326. Release 7.1.3](release-7-1-3.html)</span>

<span class="sect1">[E.327. Release 7.1.2](release-7-1-2.html)</span>

<span class="sect1">[E.328. Release 7.1.1](release-7-1-1.html)</span>

<span class="sect1">[E.329. Release 7.1](release-7-1.html)</span>

<span class="sect1">[E.330. Release 7.0.3](release-7-0-3.html)</span>

<span class="sect1">[E.331. Release 7.0.2](release-7-0-2.html)</span>

<span class="sect1">[E.332. Release 7.0.1](release-7-0-1.html)</span>

<span class="sect1">[E.333. Release 7.0](release-7-0.html)</span>

<span class="sect1">[E.334. Release 6.5.3](release-6-5-3.html)</span>

<span class="sect1">[E.335. Release 6.5.2](release-6-5-2.html)</span>

<span class="sect1">[E.336. Release 6.5.1](release-6-5-1.html)</span>

<span class="sect1">[E.337. Release 6.5](release-6-5.html)</span>

<span class="sect1">[E.338. Release 6.4.2](release-6-4-2.html)</span>

<span class="sect1">[E.339. Release 6.4.1](release-6-4-1.html)</span>

<span class="sect1">[E.340. Release 6.4](release-6-4.html)</span>

<span class="sect1">[E.341. Release 6.3.2](release-6-3-2.html)</span>

<span class="sect1">[E.342. Release 6.3.1](release-6-3-1.html)</span>

<span class="sect1">[E.343. Release 6.3](release-6-3.html)</span>

<span class="sect1">[E.344. Release 6.2.1](release-6-2-1.html)</span>

<span class="sect1">[E.345. Release 6.2](release-6-2.html)</span>

<span class="sect1">[E.346. Release 6.1.1](release-6-1-1.html)</span>

<span class="sect1">[E.347. Release 6.1](release-6-1.html)</span>

<span class="sect1">[E.348. Release 6.0](release-6-0.html)</span>

<span class="sect1">[E.349. Release 1.09](release-1-09.html)</span>

<span class="sect1">[E.350. Release 1.02](release-1-02.html)</span>

<span class="sect1">[E.351. Release 1.01](release-1-01.html)</span>

<span class="sect1">[E.352. Release 1.0](release-1-0.html)</span>

<span class="sect1">[E.353. <span class="productname">Postgres95</span>
Release 0.03](release-0-03.html)</span>

<span class="sect1">[E.354. <span class="productname">Postgres95</span>
Release 0.02](release-0-02.html)</span>

<span class="sect1">[E.355. <span class="productname">Postgres95</span>
Release 0.01](release-0-01.html)</span>

<span class="appendix">[F. Additional Supplied
Modules](contrib.html)</span>

<span class="sect1">[F.1. adminpack](adminpack.html)</span>

<span class="sect1">[F.2. amcheck](amcheck.html)</span>

<span class="sect1">[F.3. auth\_delay](auth-delay.html)</span>

<span class="sect1">[F.4. auto\_explain](auto-explain.html)</span>

<span class="sect1">[F.5. bloom](bloom.html)</span>

<span class="sect1">[F.6. btree\_gin](btree-gin.html)</span>

<span class="sect1">[F.7. btree\_gist](btree-gist.html)</span>

<span class="sect1">[F.8. chkpass](chkpass.html)</span>

<span class="sect1">[F.9. citext](citext.html)</span>

<span class="sect1">[F.10. cube](cube.html)</span>

<span class="sect1">[F.11. dblink](dblink.html)</span>

<span class="sect1">[F.12. dict\_int](dict-int.html)</span>

<span class="sect1">[F.13. dict\_xsyn](dict-xsyn.html)</span>

<span class="sect1">[F.14. earthdistance](earthdistance.html)</span>

<span class="sect1">[F.15. file\_fdw](file-fdw.html)</span>

<span class="sect1">[F.16. fuzzystrmatch](fuzzystrmatch.html)</span>

<span class="sect1">[F.17. hstore](hstore.html)</span>

<span class="sect1">[F.18. intagg](intagg.html)</span>

<span class="sect1">[F.19. intarray](intarray.html)</span>

<span class="sect1">[F.20. isn](isn.html)</span>

<span class="sect1">[F.21. lo](lo.html)</span>

<span class="sect1">[F.22. ltree](ltree.html)</span>

<span class="sect1">[F.23. pageinspect](pageinspect.html)</span>

<span class="sect1">[F.24. passwordcheck](passwordcheck.html)</span>

<span class="sect1">[F.25. pg\_buffercache](pgbuffercache.html)</span>

<span class="sect1">[F.26. pgcrypto](pgcrypto.html)</span>

<span class="sect1">[F.27. pg\_freespacemap](pgfreespacemap.html)</span>

<span class="sect1">[F.28. pg\_prewarm](pgprewarm.html)</span>

<span class="sect1">[F.29. pgrowlocks](pgrowlocks.html)</span>

<span class="sect1">[F.30.
pg\_stat\_statements](pgstatstatements.html)</span>

<span class="sect1">[F.31. pgstattuple](pgstattuple.html)</span>

<span class="sect1">[F.32. pg\_trgm](pgtrgm.html)</span>

<span class="sect1">[F.33. pg\_visibility](pgvisibility.html)</span>

<span class="sect1">[F.34. postgres\_fdw](postgres-fdw.html)</span>

<span class="sect1">[F.35. seg](seg.html)</span>

<span class="sect1">[F.36. sepgsql](sepgsql.html)</span>

<span class="sect1">[F.37. spi](contrib-spi.html)</span>

<span class="sect1">[F.38. sslinfo](sslinfo.html)</span>

<span class="sect1">[F.39. tablefunc](tablefunc.html)</span>

<span class="sect1">[F.40. tcn](tcn.html)</span>

<span class="sect1">[F.41. test\_decoding](test-decoding.html)</span>

<span class="sect1">[F.42.
tsm\_system\_rows](tsm-system-rows.html)</span>

<span class="sect1">[F.43.
tsm\_system\_time](tsm-system-time.html)</span>

<span class="sect1">[F.44. unaccent](unaccent.html)</span>

<span class="sect1">[F.45. uuid-ossp](uuid-ossp.html)</span>

<span class="sect1">[F.46. xml2](xml2.html)</span>

<span class="appendix">[G. Additional Supplied
Programs](contrib-prog.html)</span>

<span class="sect1">[G.1. Client
Applications](contrib-prog-client.html)</span>

<span class="sect1">[G.2. Server
Applications](contrib-prog-server.html)</span>

<span class="appendix">[H. External
Projects](external-projects.html)</span>

<span class="sect1">[H.1. Client
Interfaces](external-interfaces.html)</span>

<span class="sect1">[H.2. Administration
Tools](external-admin-tools.html)</span>

<span class="sect1">[H.3. Procedural Languages](external-pl.html)</span>

<span class="sect1">[H.4. Extensions](external-extensions.html)</span>

<span class="appendix">[I. The Source Code
Repository](sourcerepo.html)</span>

<span class="sect1">[I.1. Getting The Source via
<span class="productname">Git</span>](git.html)</span>

<span class="appendix">[J. Documentation](docguide.html)</span>

<span class="sect1">[J.1. DocBook](docguide-docbook.html)</span>

<span class="sect1">[J.2. Tool Sets](docguide-toolsets.html)</span>

<span class="sect1">[J.3. Building The
Documentation](docguide-build.html)</span>

<span class="sect1">[J.4. Documentation
Authoring](docguide-authoring.html)</span>

<span class="sect1">[J.5. Style Guide](docguide-style.html)</span>

<span class="appendix">[K.
Acronyms](acronyms.html)</span>

</div>

</div>

<div class="navfooter">

-----

|                                       |                    |                                                                     |
| :------------------------------------ | :----------------: | ------------------------------------------------------------------: |
| [Prev](planner-stats-security.html)   |                    |                                      [Next](errcodes-appendix.html) |
| 68.3. Planner Statistics and Security | [Home](index.html) | Appendix A. <span class="productname">PostgreSQL</span> Error Codes |

</div>
