---

---
1.创建mybatis数据库
create database mybatis default character set utf8 collate utf8_general_ci;
2.创建一个名字为country的表并插入一些简单的数据
SET FOREIGN_KEY_CHECKS=0;

-- ----------------------------
-- Table structure for country
-- ----------------------------
DROP TABLE IF EXISTS `country`;
CREATE TABLE `country` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `countryname` varchar(255) DEFAULT NULL,
  `countrycode` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of country
-- ----------------------------
INSERT INTO `country` VALUES ('1', '中国', 'CN');
INSERT INTO `country` VALUES ('2', '英国', 'GB');
INSERT INTO `country` VALUES ('3', '美国', 'US');
INSERT INTO `country` VALUES ('4', '俄罗斯', 'RU');



