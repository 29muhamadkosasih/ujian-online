-- --------------------------------------------------------
-- Host:                         127.0.0.1
-- Versi server:                 8.0.30 - MySQL Community Server - GPL
-- OS Server:                    Win64
-- HeidiSQL Versi:               12.1.0.6537
-- --------------------------------------------------------

/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET NAMES utf8 */;
/*!50503 SET NAMES utf8mb4 */;
/*!40103 SET @OLD_TIME_ZONE=@@TIME_ZONE */;
/*!40103 SET TIME_ZONE='+00:00' */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;

-- membuang struktur untuk table db_ujian_online.answers
CREATE TABLE IF NOT EXISTS `answers` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `exam_id` bigint unsigned NOT NULL,
  `exam_session_id` bigint unsigned NOT NULL,
  `question_id` bigint unsigned NOT NULL,
  `student_id` bigint unsigned NOT NULL,
  `question_order` int NOT NULL,
  `answer_order` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `answer` int NOT NULL,
  `is_correct` enum('Y','N') COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT 'N',
  `created_at` timestamp NULL DEFAULT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `answers_exam_id_foreign` (`exam_id`),
  KEY `answers_exam_session_id_foreign` (`exam_session_id`),
  KEY `answers_question_id_foreign` (`question_id`),
  KEY `answers_student_id_foreign` (`student_id`),
  CONSTRAINT `answers_exam_id_foreign` FOREIGN KEY (`exam_id`) REFERENCES `exams` (`id`) ON DELETE CASCADE,
  CONSTRAINT `answers_exam_session_id_foreign` FOREIGN KEY (`exam_session_id`) REFERENCES `exam_sessions` (`id`) ON DELETE CASCADE,
  CONSTRAINT `answers_question_id_foreign` FOREIGN KEY (`question_id`) REFERENCES `questions` (`id`) ON DELETE CASCADE,
  CONSTRAINT `answers_student_id_foreign` FOREIGN KEY (`student_id`) REFERENCES `students` (`id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Membuang data untuk tabel db_ujian_online.answers: ~0 rows (lebih kurang)

-- membuang struktur untuk table db_ujian_online.classrooms
CREATE TABLE IF NOT EXISTS `classrooms` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `title` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `created_at` timestamp NULL DEFAULT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `classrooms_title_unique` (`title`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Membuang data untuk tabel db_ujian_online.classrooms: ~2 rows (lebih kurang)
INSERT INTO `classrooms` (`id`, `title`, `created_at`, `updated_at`) VALUES
	(1, 'X RPL-1', '2023-08-20 04:55:15', '2023-08-20 04:55:15'),
	(3, 'X RPL-II', '2023-08-20 04:55:27', '2023-08-20 04:55:27');

-- membuang struktur untuk table db_ujian_online.exams
CREATE TABLE IF NOT EXISTS `exams` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `title` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `lesson_id` bigint unsigned NOT NULL,
  `classroom_id` bigint unsigned NOT NULL,
  `duration` int NOT NULL,
  `description` text COLLATE utf8mb4_unicode_ci NOT NULL,
  `random_question` enum('Y','N') COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT 'Y',
  `random_answer` enum('Y','N') COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT 'Y',
  `show_answer` enum('Y','N') COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT 'N',
  `created_at` timestamp NULL DEFAULT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `exams_lesson_id_foreign` (`lesson_id`),
  KEY `exams_classroom_id_foreign` (`classroom_id`),
  CONSTRAINT `exams_classroom_id_foreign` FOREIGN KEY (`classroom_id`) REFERENCES `classrooms` (`id`) ON DELETE CASCADE,
  CONSTRAINT `exams_lesson_id_foreign` FOREIGN KEY (`lesson_id`) REFERENCES `lessons` (`id`) ON DELETE CASCADE
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Membuang data untuk tabel db_ujian_online.exams: ~1 rows (lebih kurang)
INSERT INTO `exams` (`id`, `title`, `lesson_id`, `classroom_id`, `duration`, `description`, `random_question`, `random_answer`, `show_answer`, `created_at`, `updated_at`) VALUES
	(2, 'matematika', 3, 1, 30, '<p>tik</p>', 'Y', 'Y', 'Y', '2023-08-20 05:12:27', '2023-08-20 05:12:27');

-- membuang struktur untuk table db_ujian_online.exam_groups
CREATE TABLE IF NOT EXISTS `exam_groups` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `exam_id` bigint unsigned NOT NULL,
  `exam_session_id` bigint unsigned NOT NULL,
  `student_id` bigint unsigned NOT NULL,
  `created_at` timestamp NULL DEFAULT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `exam_groups_exam_id_foreign` (`exam_id`),
  KEY `exam_groups_exam_session_id_foreign` (`exam_session_id`),
  KEY `exam_groups_student_id_foreign` (`student_id`),
  CONSTRAINT `exam_groups_exam_id_foreign` FOREIGN KEY (`exam_id`) REFERENCES `exams` (`id`) ON DELETE CASCADE,
  CONSTRAINT `exam_groups_exam_session_id_foreign` FOREIGN KEY (`exam_session_id`) REFERENCES `exam_sessions` (`id`) ON DELETE CASCADE,
  CONSTRAINT `exam_groups_student_id_foreign` FOREIGN KEY (`student_id`) REFERENCES `students` (`id`) ON DELETE CASCADE
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Membuang data untuk tabel db_ujian_online.exam_groups: ~0 rows (lebih kurang)

-- membuang struktur untuk table db_ujian_online.exam_sessions
CREATE TABLE IF NOT EXISTS `exam_sessions` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `exam_id` bigint unsigned NOT NULL,
  `title` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `start_time` datetime NOT NULL,
  `end_time` datetime NOT NULL,
  `created_at` timestamp NULL DEFAULT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `exam_sessions_exam_id_foreign` (`exam_id`),
  CONSTRAINT `exam_sessions_exam_id_foreign` FOREIGN KEY (`exam_id`) REFERENCES `exams` (`id`) ON DELETE CASCADE
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Membuang data untuk tabel db_ujian_online.exam_sessions: ~0 rows (lebih kurang)
INSERT INTO `exam_sessions` (`id`, `exam_id`, `title`, `start_time`, `end_time`, `created_at`, `updated_at`) VALUES
	(2, 2, 'Sesi 2', '2023-08-20 12:33:00', '2023-08-20 14:33:00', '2023-08-20 05:33:32', '2023-08-20 05:33:32');

-- membuang struktur untuk table db_ujian_online.failed_jobs
CREATE TABLE IF NOT EXISTS `failed_jobs` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `uuid` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `connection` text COLLATE utf8mb4_unicode_ci NOT NULL,
  `queue` text COLLATE utf8mb4_unicode_ci NOT NULL,
  `payload` longtext COLLATE utf8mb4_unicode_ci NOT NULL,
  `exception` longtext COLLATE utf8mb4_unicode_ci NOT NULL,
  `failed_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  UNIQUE KEY `failed_jobs_uuid_unique` (`uuid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Membuang data untuk tabel db_ujian_online.failed_jobs: ~0 rows (lebih kurang)

-- membuang struktur untuk table db_ujian_online.grades
CREATE TABLE IF NOT EXISTS `grades` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `exam_id` bigint unsigned NOT NULL,
  `exam_session_id` bigint unsigned NOT NULL,
  `student_id` bigint unsigned NOT NULL,
  `duration` int NOT NULL,
  `start_time` datetime DEFAULT NULL,
  `end_time` datetime DEFAULT NULL,
  `total_correct` int NOT NULL,
  `grade` decimal(5,2) NOT NULL,
  `created_at` timestamp NULL DEFAULT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `grades_exam_id_foreign` (`exam_id`),
  KEY `grades_exam_session_id_foreign` (`exam_session_id`),
  KEY `grades_student_id_foreign` (`student_id`),
  CONSTRAINT `grades_exam_id_foreign` FOREIGN KEY (`exam_id`) REFERENCES `exams` (`id`) ON DELETE CASCADE,
  CONSTRAINT `grades_exam_session_id_foreign` FOREIGN KEY (`exam_session_id`) REFERENCES `exam_sessions` (`id`) ON DELETE CASCADE,
  CONSTRAINT `grades_student_id_foreign` FOREIGN KEY (`student_id`) REFERENCES `students` (`id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Membuang data untuk tabel db_ujian_online.grades: ~0 rows (lebih kurang)

-- membuang struktur untuk table db_ujian_online.lessons
CREATE TABLE IF NOT EXISTS `lessons` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `title` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `created_at` timestamp NULL DEFAULT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `lessons_title_unique` (`title`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Membuang data untuk tabel db_ujian_online.lessons: ~0 rows (lebih kurang)
INSERT INTO `lessons` (`id`, `title`, `created_at`, `updated_at`) VALUES
	(1, 'Bahasa Indonesia', '2023-08-20 04:41:59', '2023-08-20 04:41:59'),
	(3, 'Teknik Informatika dan Komunikasi', '2023-08-20 04:42:31', '2023-08-20 04:45:33');

-- membuang struktur untuk table db_ujian_online.migrations
CREATE TABLE IF NOT EXISTS `migrations` (
  `id` int unsigned NOT NULL AUTO_INCREMENT,
  `migration` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `batch` int NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=15 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Membuang data untuk tabel db_ujian_online.migrations: ~0 rows (lebih kurang)
INSERT INTO `migrations` (`id`, `migration`, `batch`) VALUES
	(1, '2014_10_12_000000_create_users_table', 1),
	(2, '2014_10_12_100000_create_password_resets_table', 1),
	(3, '2019_08_19_000000_create_failed_jobs_table', 1),
	(4, '2019_12_14_000001_create_personal_access_tokens_table', 1),
	(5, '2023_08_19_164007_create_lessons_table', 1),
	(6, '2023_08_19_164213_create_classrooms_table', 1),
	(7, '2023_08_19_164309_create_exams_table', 1),
	(8, '2023_08_19_164355_create_students_table', 1),
	(9, '2023_08_19_164452_create_questions_table', 1),
	(10, '2023_08_19_164612_create_exam_sessions_table', 1),
	(11, '2023_08_19_164650_create_exam_groups_table', 1),
	(12, '2023_08_19_164746_create_answers_table', 1),
	(13, '2023_08_19_164828_create_grades_table', 1),
	(14, '2014_10_12_200000_add_two_factor_columns_to_users_table', 2);

-- membuang struktur untuk table db_ujian_online.password_resets
CREATE TABLE IF NOT EXISTS `password_resets` (
  `email` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `token` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `created_at` timestamp NULL DEFAULT NULL,
  KEY `password_resets_email_index` (`email`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Membuang data untuk tabel db_ujian_online.password_resets: ~0 rows (lebih kurang)

-- membuang struktur untuk table db_ujian_online.personal_access_tokens
CREATE TABLE IF NOT EXISTS `personal_access_tokens` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `tokenable_type` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `tokenable_id` bigint unsigned NOT NULL,
  `name` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `token` varchar(64) COLLATE utf8mb4_unicode_ci NOT NULL,
  `abilities` text COLLATE utf8mb4_unicode_ci,
  `last_used_at` timestamp NULL DEFAULT NULL,
  `created_at` timestamp NULL DEFAULT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `personal_access_tokens_token_unique` (`token`),
  KEY `personal_access_tokens_tokenable_type_tokenable_id_index` (`tokenable_type`,`tokenable_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Membuang data untuk tabel db_ujian_online.personal_access_tokens: ~0 rows (lebih kurang)

-- membuang struktur untuk table db_ujian_online.questions
CREATE TABLE IF NOT EXISTS `questions` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `exam_id` bigint unsigned NOT NULL,
  `question` text COLLATE utf8mb4_unicode_ci NOT NULL,
  `option_1` text COLLATE utf8mb4_unicode_ci,
  `option_2` text COLLATE utf8mb4_unicode_ci,
  `option_3` text COLLATE utf8mb4_unicode_ci,
  `option_4` text COLLATE utf8mb4_unicode_ci,
  `option_5` text COLLATE utf8mb4_unicode_ci,
  `answer` int NOT NULL,
  `created_at` timestamp NULL DEFAULT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `questions_exam_id_foreign` (`exam_id`),
  CONSTRAINT `questions_exam_id_foreign` FOREIGN KEY (`exam_id`) REFERENCES `exams` (`id`) ON DELETE CASCADE
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Membuang data untuk tabel db_ujian_online.questions: ~0 rows (lebih kurang)
INSERT INTO `questions` (`id`, `exam_id`, `question`, `option_1`, `option_2`, `option_3`, `option_4`, `option_5`, `answer`, `created_at`, `updated_at`) VALUES
	(2, 2, 'Berapakah hasil dari 2 + 2 ?…', '2', '3', '4', '5', '6', 3, '2023-08-20 05:21:50', '2023-08-20 05:21:50'),
	(3, 2, 'Berapakah hasil dari 2 x 5 ?…', '10', '11', '15', '35', '46', 1, '2023-08-20 05:21:50', '2023-08-20 05:21:50');

-- membuang struktur untuk table db_ujian_online.students
CREATE TABLE IF NOT EXISTS `students` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `classroom_id` bigint unsigned NOT NULL,
  `nisn` bigint NOT NULL,
  `name` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `password` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `gender` enum('L','P') COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT 'L',
  `created_at` timestamp NULL DEFAULT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `students_nisn_unique` (`nisn`),
  KEY `students_classroom_id_foreign` (`classroom_id`),
  CONSTRAINT `students_classroom_id_foreign` FOREIGN KEY (`classroom_id`) REFERENCES `classrooms` (`id`) ON DELETE CASCADE
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Membuang data untuk tabel db_ujian_online.students: ~2 rows (lebih kurang)
INSERT INTO `students` (`id`, `classroom_id`, `nisn`, `name`, `password`, `gender`, `created_at`, `updated_at`) VALUES
	(3, 1, 593478321193, 'Fika Ridaul Maulayya', 'password', 'L', '2023-08-20 05:03:45', '2023-08-20 05:03:45'),
	(4, 1, 623478325734, 'Akhmad Lutfi', 'password', 'L', '2023-08-20 05:03:45', '2023-08-20 05:03:45');

-- membuang struktur untuk table db_ujian_online.users
CREATE TABLE IF NOT EXISTS `users` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `email` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `email_verified_at` timestamp NULL DEFAULT NULL,
  `password` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `two_factor_secret` text COLLATE utf8mb4_unicode_ci,
  `two_factor_recovery_codes` text COLLATE utf8mb4_unicode_ci,
  `two_factor_confirmed_at` timestamp NULL DEFAULT NULL,
  `remember_token` varchar(100) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  `created_at` timestamp NULL DEFAULT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `users_email_unique` (`email`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Membuang data untuk tabel db_ujian_online.users: ~0 rows (lebih kurang)
INSERT INTO `users` (`id`, `name`, `email`, `email_verified_at`, `password`, `two_factor_secret`, `two_factor_recovery_codes`, `two_factor_confirmed_at`, `remember_token`, `created_at`, `updated_at`) VALUES
	(1, 'Administrator', 'admin@gmail.com', NULL, '$2y$10$h6rZ7vgOC4Sftu2TgL4ISueOU2I21NEwzsunaEjhOTACiQz0VKRYy', NULL, NULL, NULL, NULL, '2023-08-19 09:55:06', '2023-08-19 09:55:06');

/*!40103 SET TIME_ZONE=IFNULL(@OLD_TIME_ZONE, 'system') */;
/*!40101 SET SQL_MODE=IFNULL(@OLD_SQL_MODE, '') */;
/*!40014 SET FOREIGN_KEY_CHECKS=IFNULL(@OLD_FOREIGN_KEY_CHECKS, 1) */;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40111 SET SQL_NOTES=IFNULL(@OLD_SQL_NOTES, 1) */;
