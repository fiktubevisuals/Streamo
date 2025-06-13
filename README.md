<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Streamo</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            overflow-x: hidden; /* Prevent horizontal scrolling */
        }
        .material-icons {
            font-family: 'Material Icons';
            font-weight: normal;
            font-style: normal;
            font-size: 24px;  /* Preferred size of the icon */
            display: inline-block;
            line-height: 1;
            text-transform: none;
            letter-spacing: normal;
            word-wrap: normal;
            white-space: nowrap;
            direction: ltr;
            -webkit-font-smoothing: antialiased; /* Support for all WebKit browsers. */
            text-rendering: optimizeLegibility; /* Support for Safari and Chrome. */
            -moz-osx-font-smoothing: grayscale; /* Support for Firefox. */
            font-feature-settings: 'liga'; /* Support for IE. */
        }
        .verified-badge {
            color: #34d399; /* Tailwind: text-green-400 */
            font-size: 1em; /* Relative to parent text size */
            margin-left: 0.25rem; /* Tailwind: ml-1 */
        }

        .video-card, .music-card {
            background-color: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 0.5rem;  
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            cursor: pointer;
            overflow: hidden;  
        }
        .video-card:hover, .music-card:hover {
            transform: scale(1.01);
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.2), 8px 10px -6px rgba(0, 0, 0, 0.1);
        }
        .video-card-thumbnail, .music-card-thumbnail {
            width: 100%;
            height: 200px;  
            object-fit: cover;
        }
        .video-card-content, .music-card-content {
            padding: 1rem;  
        }
        .video-card-title, .music-card-title {
            font-size: 1.125rem;  
            font-weight: 600;  
            color: white;
            margin-bottom: 0.5rem;  
            display: -webkit-box;
            -webkit-line-clamp: 1;
            -webkit-box-orient: vertical;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        .video-card-description, .music-card-artist {
            color: #d1d5db;  
            font-size: 0.875rem;  
            margin-bottom: 0.75rem;  
            display: -webkit-box;
            -webkit-line-clamp: 2;  
            -webkit-box-orient: vertical;
            overflow: hidden;
            text-overflow: ellipsis;
            min-height: 2.625rem;  
        }
        .video-card-meta, .music-card-meta {
            color: #9ca3af;  
            font-size: 0.75rem;  
            display: flex;
            align-items: center;
            gap: 0.5rem;  
            flex-wrap: wrap; 
        }
        .video-card-meta span:nth-child(2), .music-card-meta .artist-name-meta {  
            font-weight: 500;
            color: #a78bfa;  
            cursor: pointer;  
        }
        .video-card-meta span:nth-child(2):hover, .music-card-meta .artist-name-meta:hover {
            text-decoration: underline;
        }
        .music-card-meta .download-icon {
            margin-left: auto; 
            color: #9ca3af;
        }
        .music-card-meta .download-icon:hover {
            color: white;
        }

        /* TikTok-like Video Player Styles */
        .tiktok-video-container {
            position: relative;
            width: 100%;  
            height: 100%; 
            background-color: #000;
            display: flex;
            justify-content: center;
            align-items: center;
            scroll-snap-align: start;  
        }
        #tiktok-videos-section { 
            height: calc(100vh - var(--header-height, 68px) - var(--footer-height, 60px)); 
        }

        .tiktok-video-player {
            width: 100%;  
            height: 100%;  
            object-fit: cover;  
            display: block;
        }
        .tiktok-video-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;  
            padding: 1rem;
            background: linear-gradient(to top, rgba(0,0,0,0.6) 0%, rgba(0,0,0,0) 20%);  
            pointer-events: none;  
        }
        .tiktok-video-info {
            color: white;
            pointer-events: auto;  
            margin-bottom: 1rem;
        }
        .tiktok-video-info h3 {
            font-size: 1.25rem;  
            font-weight: 600;
            margin-bottom: 0.25rem;
        }
        .tiktok-video-info p {
            font-size: 0.875rem;  
            color: #d1d5db;
        }
        .tiktok-video-info .uploader-name {  
            font-weight: 500;
            color: #a78bfa;  
            cursor: pointer;  
        }
        .tiktok-video-info .uploader-name:hover {
            text-decoration: underline;
        }
        .tiktok-video-actions {
            position: absolute;
            bottom: 1rem;
            right: 1rem;
            display: flex;
            flex-direction: column;
            gap: 1rem; 
            pointer-events: auto;  
        }
        .tiktok-action-button {
            display: flex;
            flex-direction: column;
            align-items: center;
            color: white;
            font-size: 0.75rem;  
            font-weight: 500;
            background: none;
            border: none;
            cursor: pointer;
            padding: 0.5rem;
            border-radius: 0.375rem;
            transition: background-color 0.2s ease;
        }
        .tiktok-action-button:hover {
            background-color: rgba(255,255,255,0.1);
        }
        .tiktok-action-button .material-icons {
            font-size: 2rem;  
            margin-bottom: 0.25rem;
        }

        /* YouTube-like Video Player Styles (for Movies section) */
        .movie-player-container {
            position: relative;
            width: 100%;
            background-color: #000;  
            border-radius: 0.5rem;
        }
        .movie-player {
            width: 100%;
            border-radius: 0.5rem;
            display: block;  
            aspect-ratio: 16 / 9;  
        }
        .movie-details {
            padding: 1rem 0;
        }
        .movie-title {
            font-size: 1.75rem;  
            font-weight: 700;  
            color: white;
            margin-bottom: 0.5rem;
        }
        .movie-meta-info {
            color: #9ca3af;  
            font-size: 0.875rem;  
            margin-bottom: 1rem;
        }
        .movie-description {
            color: #d1d5db;  
            font-size: 1rem;  
            line-height: 1.6;
            margin-bottom: 1.5rem;
        }
        .movie-interactions {
            display: flex;
            align-items: center;
            gap: 0.75rem; 
            margin-top: 1rem;
            color: #d1d5db;
            font-size: 0.875rem;
            flex-wrap: wrap;  
        }
        .movie-interaction-button {
            display: flex;
            align-items: center;
            gap: 0.375rem;  
            background: rgba(255,255,255,0.08);
            border: 1px solid rgba(255,255,255,0.1);
            color: inherit;
            cursor: pointer;
            padding: 0.5rem 0.75rem;  
            border-radius: 0.375rem;  
            transition: background-color 0.2s ease;
            font-weight: 500;  
        }
        .movie-interaction-button:hover {
            background-color: rgba(255,255,255,0.15);
        }
        .movie-interaction-button .material-icons {
            font-size: 1.25rem;  
        }
        .movie-comments-section {
            margin-top: 2rem;
        }
        .movie-comment {
            background-color: rgba(255,255,255,0.05);
            padding: 0.75rem;
            border-radius: 0.375rem;
            margin-bottom: 0.75rem;
            border: 1px solid rgba(255,255,255,0.08);
        }
        .movie-comment strong {
            color: white;
            font-weight: 500;  
        }
        .movie-comment p {
            font-size: 0.875rem;  
            color: #d1d5db;  
            margin-top: 0.25rem;
        }
        .movie-comment-form {
            display: flex;
            gap: 0.5rem;
            margin-top: 1rem;
        }
        .movie-comment-form input {
            flex-grow: 1;
            padding: 0.5rem 0.75rem;
            border-radius: 0.375rem;
            background-color: rgba(0, 0, 0, 0.3);
            color: white;
            border: 1px solid #4b5563;  
        }
        .movie-comment-form button {
            padding: 0.5rem 1rem;
            background-color: #3b82f6;  
            color: white;
            border-radius: 0.375rem;
            cursor: pointer;
            font-weight: 500;
        }
        .movie-comment-form button:hover {
            background-color: #2563eb;  
        }

        /* Music Section Specific Styles */
        .music-now-playing-bar {
            position: sticky;
            bottom: 0;
            left: 0;
            width: 100%;
            background-color: rgba(17, 24, 39, 0.9);  
            backdrop-filter: blur(8px);
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            padding: 0.75rem 1.5rem;  
            display: flex;
            align-items: center;
            justify-content: space-between;
            z-index: 50;  
        }
        .music-now-playing-bar img {
            width: 40px;  
            height: 40px;  
            border-radius: 0.25rem;  
            object-fit: cover;
        }
        .music-controls button .material-icons {
            font-size: 1.75rem;  
        }

        /* Modal Styles */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0,0,0,0.7);
            backdrop-filter: blur(5px);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 999;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.3s ease, visibility 0.3s ease;
        }
        .modal-overlay.open {
            opacity: 1;
            visibility: visible;
        }
        .modal-content {
            background-color: rgba(31, 41, 55, 0.8);  
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 0.75rem;  
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.2), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
            padding: 2rem;  
            width: 90%;
            max-width: 450px;
            max-height: 90vh;
            overflow-y: auto;
            position: relative;  
        }
        .modal-close-button {
            position: absolute;
            top: 1rem;
            right: 1rem;
            background: none;
            border: none;
            color: #9ca3af;  
            font-size: 1.5rem;  
            cursor: pointer;
        }
        .modal-close-button:hover {
            color: white;
        }
        .modal-header {
            margin-bottom: 1.5rem;  
            text-align: center;
        }
        .modal-title {
            font-size: 1.5rem;  
            font-weight: 600;  
            color: white;
            margin-bottom: 0.5rem;  
        }
        .modal-description {
            color: #d1d5db;  
            font-size: 0.875rem;  
        }
        .form-group {
            margin-bottom: 1.5rem;  
        }
        .form-label {
            display: block;
            font-size: 0.875rem;  
            font-weight: 500;  
            color: white;
            margin-bottom: 0.5rem;  
        }
        .form-input, .form-textarea, .form-select {  
            width: 100%;
            padding: 0.75rem 1rem;  
            border-radius: 0.375rem;  
            background-color: rgba(0, 0, 0, 0.3);
            color: white;
            border: 1px solid #4b5563;  
            font-size: 1rem;  
            transition: border-color 0.2s ease, box-shadow 0.2s ease;
        }
        .form-select {  
            appearance: none;  
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' fill='none' viewBox='0 0 20 20'%3E%3Cpath stroke='%239ca3af' stroke-linecap='round' stroke-linejoin='round' stroke-width='1.5' d='M6 8l4 4 4-4'/%3E%3C/svg%3E");
            background-position: right 0.75rem center;
            background-repeat: no-repeat;
            background-size: 1.25em 1.25em;
            padding-right: 2.5rem;  
        }
        .form-input:focus, .form-textarea:focus, .form-select:focus {
            outline: none;
            border-color: #6b7280;  
            box-shadow: 0 0 0 3px rgba(107, 114, 128, 0.2);  
        }
        .form-textarea {
            min-height: 100px;
            resize: vertical;
        }
        .file-info {
            font-size: 0.75rem;  
            color: #d1d5db;  
            margin-top: 0.25rem;  
        }
         .watermark-info { /* New style for watermark info text */
            font-size: 0.75rem;
            color: #9ca3af; /* gray-400 */
            margin-top: 0.5rem;
            text-align: center;
        }
        .form-error, .form-success {
            font-size: 0.875rem;  
            margin-top: 0.5rem;  
            display: flex;
            align-items: center;
            gap: 0.5rem;  
            padding: 0.75rem;  
            border-radius: 0.375rem;  
        }
        .form-error {
            color: #f87171;  
            background-color: rgba(239, 68, 68, 0.1);  
            border: 1px solid rgba(239, 68, 68, 0.3);
        }
        .form-success {
            color: #34d399;  
            background-color: rgba(16, 185, 129, 0.1);  
            border: 1px solid rgba(16, 185, 129, 0.3);
        }
        .form-buttons {
            display: flex;
            justify-content: flex-end;
            gap: 0.75rem;  
            margin-top: 2rem;  
        }
        .form-button {
            padding: 0.75rem 1.5rem;  
            border-radius: 0.375rem;  
            font-weight: 500;  
            cursor: pointer;
            transition: background-color 0.2s ease, opacity 0.2s ease;
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;  
        }
        .form-button-cancel {
            background-color: #4b5563;  
        }
        .form-button-cancel:hover {
            background-color: #6b7280;  
        }
        .form-button-submit {  
            background-color: #3b82f6;  
        }
        .form-button-submit:hover {
            background-color: #2563eb;  
        }
        .form-button-submit.submitting {
            opacity: 0.7;
            cursor: not-allowed;
        }
        .gift-button-send {  
            background-color: #16a34a;  
        }
        .gift-button-send:hover {
            background-color: #15803d;  
        }
        .gift-button-send.sending {
            opacity: 0.7;
            cursor: not-allowed;
        }
        .withdraw-button-send {  
            background-color: #f97316;  
        }
        .withdraw-button-send:hover {
            background-color: #ea580c;  
        }
        .withdraw-button-send.sending {
            opacity: 0.7;
            cursor: not-allowed;
        }
        .creator-wallet-info {
            margin-top: 1rem;  
            padding: 1rem;  
            border-radius: 0.375rem;  
            background-color: rgba(0, 0, 0, 0.2);
            color: #d1d5db;  
            font-size: 0.875rem;  
            border: 1px solid rgba(255,255,255,0.1);
        }
        .wallet-section {
            margin-top: 2rem;
            padding: 1.5rem;
            background-color: rgba(55, 65, 81, 0.7);  
            border-radius: 0.5rem;  
            border: 1px solid rgba(255, 255, 255, 0.08);
        }
        .wallet-header {  
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 1rem;
            margin-bottom: 1.5rem;  
        }
        .wallet-section h3 {
            font-size: 1.5rem;  
            font-weight: 600;  
            color: white;
        }
        .wallet-balance-container {  
            text-align: left;  
            margin-bottom: 1.5rem;
        }
        .wallet-balance-label {
            font-size: 1rem;  
            color: #9ca3af;  
            margin-bottom: 0.25rem;
        }
        .wallet-balance {
            font-size: 2.25rem;  
            font-weight: 700;  
            color: #34d399;  
        }
        .wallet-actions {
            display: flex;
            gap: 0.75rem;  
            flex-wrap: wrap;  
        }

        /* Mobile Menu Specific Styles */
        .mobile-menu-container {
            display: none;  
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(17, 24, 39, 0.95);  
            backdrop-filter: blur(10px);
            z-index: 1000;
            overflow-y: auto;
            padding-top: 4rem;  
        }
        .mobile-menu-container.open {
            display: block;
        }
        .mobile-menu-content {
            padding: 1rem 2rem;  
            display: flex;
            flex-direction: column;
            align-items: flex-start;  
            gap: 0.5rem;  
        }
        .mobile-menu-close-btn {  
            position: absolute;
            top: 1.5rem;  
            right: 1.5rem;  
            color: white;
            font-size: 2rem;  
            cursor: pointer;
            background: none;
            border: none;
        }
        .mobile-menu-link {
            color: #d1d5db;  
            font-size: 1.125rem;  
            font-weight: 500;  
            text-decoration: none;
            transition: color 0.2s ease, background-color 0.2s ease;
            padding: 0.75rem 1rem;  
            border-radius: 0.375rem;  
            width: 100%;
            text-align: left;
            display: flex;
            align-items: center;
            gap: 0.75rem;  
        }
        .mobile-menu-link:hover {
            color: white;
            background-color: rgba(255, 255, 255, 0.1);
        }
        .mobile-menu-link.active {
            color: white;
            background-color: #8b5cf6;  
        }
        .mobile-menu-button-group {
            margin-top: 1.5rem;  
            width: 100%;
            display: flex;
            flex-direction: column;
            gap: 0.75rem;  
        }
        .mobile-menu-button {  
            padding: 0.75rem 1rem;
            border-radius: 0.375rem;
            font-weight: 500;
            cursor: pointer;
            transition: background-color 0.2s ease;
            color: white;
            width: 100%;
            text-align: left;
            display: flex;
            align-items: center;
            gap: 0.75rem;
        }
        .btn {
            padding: 0.5rem 1rem;  
            border-radius: 0.375rem;  
            font-weight: 500;  
            transition: background-color 0.2s ease;
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;  
        }
        .btn-primary {
            background-color: #3b82f6;  
            color: white;
        }
        .btn-primary:hover {
            background-color: #2563eb;  
        }
        .btn-secondary {
            background-color: #4b5563;  
            color: white;
        }
        .btn-secondary:hover {
            background-color: #6b7280;  
        }
        .nav-link {
            padding: 0.5rem 1rem;
            border-radius: 0.375rem;
            color: #d1d5db;  
            font-weight: 500;  
            transition: color 0.2s ease, background-color 0.2s ease, border-color 0.2s ease;  
            border-bottom: 2px solid transparent;  
        }
        .nav-link:hover {
            color: white;
            background-color: rgba(255,255,255,0.05);  
        }
        .nav-link.active {
            color: white;
            border-bottom-color: #8b5cf6;  
            background-color: transparent;  
            border-radius: 0;  
        }

        /* Social Login Button Styles */
        .btn-social {
            padding: 0.75rem 1.5rem;
            border-radius: 0.375rem;
            font-weight: 500;
            cursor: pointer;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            display: inline-flex; 
            align-items: center;
            justify-content: center;
            gap: 0.75rem; 
            width: 100%; 
            text-align: center;
        }
        .btn-social svg, .btn-social .material-icons { 
            width: 20px;
            height: 20px;
        }

        .btn-google {
            background-color: #ffffff;
            color: #374151;  
            border: 1px solid #d1d5db; 
        }
        .btn-google:hover {
            background-color: #f9fafb; 
            box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
        }
        .btn-facebook {
            background-color: #1877F2; 
            color: white;
        }
        .btn-facebook:hover {
            background-color: #166FE5;
        }
        .btn-tiktok {
            background-color: #000000; 
            color: white;
            border: 1px solid #4b5563; 
        }
        .btn-tiktok:hover {
            background-color: #111111; 
        }
        .btn-x { 
            background-color: #000000; 
            color: white;
            border: 1px solid #4b5563; 
        }
        .btn-x:hover {
            background-color: #111111;
        }
        .btn-social .material-icons { 
             font-size: 20px;
        }

        /* Plus button specific styles */
        .btn-plus {
            width: 40px;  
            height: 40px;
            border-radius: 50%;  
            background-color: #8b5cf6;  
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.75rem;  
            font-weight: 300;  
            line-height: 1;  
            padding: 0;  
            transition: background-color 0.2s ease;
            border: none;  
        }
        .btn-plus:hover {
            background-color: #7c3aed;  
        }

        /* Profile Section Specific Styles */
        .profile-header {
            display: flex;
            align-items: center;
            gap: 1.5rem;  
            margin-bottom: 2rem;  
            padding-bottom: 2rem;  
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }
        .profile-avatar {
            width: 100px;  
            height: 100px;  
            border-radius: 50%;
            object-fit: cover;
            border: 3px solid #8b5cf6;  
        }
        .profile-info h2 { 
            font-size: 1.875rem;  
            font-weight: 600;  
            color: white;
            display: flex; 
            align-items: center;
        }
        .profile-info p {
            font-size: 1rem;  
            color: #d1d5db;  
        }
        .profile-stats {
            display: flex;
            gap: 1rem;  
            margin-top: 0.5rem;  
        }
        .profile-stats span {
            font-size: 0.875rem;  
            color: #9ca3af;  
        }
        .profile-stats strong {
            color: white;
        }
        .profile-tabs {
            margin-bottom: 1.5rem;  
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }
        .profile-tab-button {
            padding: 0.75rem 1rem;  
            font-weight: 500;  
            color: #d1d5db;  
            border-bottom: 2px solid transparent;
            transition: color 0.2s ease, border-color 0.2s ease;
        }
        .profile-tab-button:hover {
            color: white;
        }
        .profile-tab-button.active {
            color: #8b5cf6;  
            border-bottom-color: #8b5cf6;  
        }
        .profile-content-section {
            display: none;  
        }
        .profile-content-section.active {
            display: block;
        }
        /* Styles for image previews in forms */
        .image-preview {
            width: 100px;
            height: 100px;
            border-radius: 0.375rem;  
            object-fit: cover;
            margin-top: 0.5rem;  
            background-color: rgba(0,0,0,0.2);
            border: 1px dashed #4b5563;  
        }
        .banner-preview {
            width: 100%;
            max-height: 150px;
            border-radius: 0.375rem;  
            object-fit: cover;
            margin-top: 0.5rem;  
            background-color: rgba(0,0,0,0.2);
            border: 1px dashed #4b5563;  
        }

        /* Live Studio Specific Styles */
        .live-studio-video-preview {
            width: 100%;
            max-width: 720px; 
            aspect-ratio: 16 / 9;
            background-color: #000;
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 0.5rem;
            margin-bottom: 1rem;
            object-fit: cover; 
        }
        .live-status-indicator {
            padding: 0.25rem 0.75rem;
            border-radius: 0.375rem;
            font-weight: 600;
            text-transform: uppercase;
            font-size: 0.75rem;
        }
        .live-status-indicator.live {
            background-color: #ef4444; /* red-500 */
            color: white;
        }
        .live-status-indicator.offline {
            background-color: #6b7280; /* gray-500 */
            color: white;
        }
        .live-studio-controls .form-input,
        .live-studio-controls .form-textarea {
            background-color: rgba(255,255,255,0.05);
            border-color: rgba(255,255,255,0.2);
        }
        .live-studio-controls .form-input:focus,
        .live-studio-controls .form-textarea:focus {
            border-color: #8b5cf6;
            box-shadow: 0 0 0 3px rgba(139, 92, 246, 0.3);
        }
        .live-studio-button {
            padding: 0.75rem 1.5rem;
            border-radius: 0.375rem;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.2s ease, opacity 0.2s ease;
            color: white;
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
        }
        .live-studio-button .material-icons {
            font-size: 1.25rem;
        }
        .btn-go-live {
            background-color: #ef4444; /* red-500 */
        }
        .btn-go-live:hover {
            background-color: #dc2626; /* red-600 */
        }
        .btn-stop-stream {
            background-color: #6b7280; /* gray-500 */
        }
        .btn-stop-stream:hover {
            background-color: #4b5563; /* gray-600 */
        }
        .live-studio-placeholder-text {
            color: #9ca3af;
            font-style: italic;
        }

    </style>
</head>
<body class="bg-gradient-to-br from-gray-900 via-purple-900 to-black text-white flex flex-col min-h-screen">
    <header class="sticky top-0 z-50 bg-black/80 backdrop-blur-md border-b border-white/10 py-4">
        <div class="container mx-auto px-4 flex items-center justify-between">
            <a href="#" id="logo-link" class="text-2xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-blue-400 to-purple-400">
                Streamo
            </a>
            <nav class="hidden md:flex items-center gap-1"> 
                <button data-section="home-section" class="nav-link">Home</button>
                <button data-section="tiktok-videos-section" class="nav-link active">Videos</button> 
                <button data-section="movies-section" class="nav-link">Movies</button>
                <button data-section="music-section" class="nav-link">Music</button> 
                <button data-section="live-streams-section" class="nav-link">Live Streams</button>
                <button data-section="profile-section" class="nav-link">Profile</button>
                <button data-section="live-studio-section" class="nav-link">Live Studio</button>
            </nav>

            <div id="auth-buttons-desktop" class="hidden md:flex items-center gap-3">
                <button id="signin-button-desktop" class="btn btn-secondary">Sign In</button>
                <button id="signup-button-desktop" class="btn btn-primary">Sign Up</button>
            </div>

            <div id="user-area-desktop" class="hidden md:flex items-center gap-3"> 
                <div id="user-dropdown" class="relative">
                    <button id="user-dropdown-button" class="flex items-center gap-2 text-gray-300 hover:text-white p-1 rounded-md hover:bg-white/10">
                        <img id="user-avatar-nav" src="https://placehold.co/40x40/7c3aed/ffffff?text=U" alt="User Avatar" class="w-8 h-8 rounded-full object-cover border-2 border-transparent group-hover:border-purple-400">
                        <span id="user-name-nav" class="hidden lg:inline text-sm">Alice Smith</span>
                        <span id="user-verified-badge-nav" class="hidden material-icons verified-badge text-sm">verified</span>
                        <span class="material-icons text-base">expand_more</span>
                    </button>
                    <div id="dropdown-menu" class="absolute right-0 mt-2 w-56 bg-gray-800/90 backdrop-blur-md border border-white/10 rounded-md shadow-lg hidden origin-top-right z-50">
                        <div class="px-4 py-3">
                            <p class="text-sm text-white font-medium flex items-center" id="dropdown-user-name-container">
                                <span id="dropdown-user-name">Alice Smith</span>
                                <span id="dropdown-user-verified-badge" class="hidden material-icons verified-badge text-sm">verified</span>
                            </p>
                            <p class="text-xs text-gray-400" id="dropdown-user-email">alice@example.com</p>
                        </div>
                        <hr class="border-white/10 my-1">
                        <button data-section="profile-section" data-tab="settings" class="profile-tab-changer w-full text-left px-4 py-2 text-sm text-gray-200 hover:bg-white/10 hover:text-white rounded-md flex items-center gap-2">
                            <span class="material-icons text-lg">settings</span> Settings
                        </button>
                         <button data-section="profile-section" data-tab="wallet" class="profile-tab-changer w-full text-left px-4 py-2 text-sm text-gray-200 hover:bg-white/10 hover:text-white rounded-md flex items-center gap-2">
                            <span class="material-icons text-lg">account_balance_wallet</span> Wallet
                        </button>
                        <hr class="border-white/10 my-1">
                        <button id="logout-link" class="w-full text-left px-4 py-2 text-sm text-gray-200 hover:bg-red-500/20 hover:text-red-400 rounded-md flex items-center gap-2">
                            <span class="material-icons text-lg">logout</span> Logout
                        </button>
                    </div>
                </div>
                <button id="upload-button" class="btn-plus" title="Upload Content">
                    <span class="material-icons text-2xl">add</span>
                </button>
            </div>

            <div class="md:hidden">
                <button id="mobile-menu-open-button" class="text-gray-300 hover:text-white p-2 rounded-md hover:bg-white/10">
                    <span class="material-icons text-2xl">menu</span>
                </button>
            </div>
        </div>
    </header>

    <div id="mobile-menu-container" class="mobile-menu-container">
        <button id="mobile-menu-close-button" class="mobile-menu-close-btn">&times;</button>
        <div class="mobile-menu-content">
            <a href="#" data-section="home-section" class="mobile-menu-link"><span class="material-icons">home</span>Home</a>
            <a href="#" data-section="tiktok-videos-section" class="mobile-menu-link active"><span class="material-icons">ondemand_video</span>Videos</a>
            <a href="#" data-section="movies-section" class="mobile-menu-link"><span class="material-icons">movie</span>Movies</a>
            <a href="#" data-section="music-section" class="mobile-menu-link"><span class="material-icons">music_note</span>Music</a> 
            <a href="#" data-section="live-streams-section" class="mobile-menu-link"><span class="material-icons">sensors</span>Live Streams</a>
            <a href="#" data-section="profile-section" class="mobile-menu-link"><span class="material-icons">account_circle</span>Profile</a>
            <a href="#" data-section="live-studio-section" class="mobile-menu-link"><span class="material-icons">videocam</span>Live Studio</a>

            <div id="auth-buttons-mobile" class="mobile-menu-button-group">
                <button id="signin-button-mobile" class="mobile-menu-button bg-gray-700 hover:bg-gray-600"><span class="material-icons">login</span>Sign In</button>
                <button id="signup-button-mobile" class="mobile-menu-button bg-blue-600 hover:bg-blue-500"><span class="material-icons">person_add</span>Sign Up</button>
            </div>
            <div id="user-area-mobile" class="mobile-menu-button-group">
                <button id="upload-button-mobile" class="mobile-menu-button bg-purple-600 hover:bg-purple-500"><span class="material-icons">add_circle_outline</span>Upload Content</button>
                <button id="logout-link-mobile" class="mobile-menu-button bg-red-600 hover:bg-red-500"><span class="material-icons">logout</span>Logout</button>
            </div>
        </div>
    </div>

    <main class="flex-grow container mx-auto px-4 py-2 md:py-6">
        <section id="home-section" class="main-section hidden">
            <h1 class="text-3xl font-bold mb-6 text-center">Welcome to Streamo!</h1>
            <p class="text-lg text-gray-300 text-center">Discover amazing videos, movies, music, and live streams.</p>
        </section>

        <section id="tiktok-videos-section" class="main-section overflow-y-auto scroll-smooth snap-y snap-mandatory -mx-4 md:-mx-0">
            <div class="tiktok-video-container">
                <video class="tiktok-video-player" src="https://placehold.co/1080x1920.mp4?text=Video+1" loop muted playsinline></video>
                <div class="tiktok-video-overlay">
                    <div class="tiktok-video-info">
                        <h3>Epic Fail Compilation #23</h3>
                        <p>Uploaded by <span class="uploader-name">FunnyMoments</span> <span class="material-icons verified-badge">verified</span></p>
                    </div>
                </div>
                <div class="tiktok-video-actions">
                    <button class="tiktok-action-button"><span class="material-icons">favorite_border</span>1.2M</button>
                    <button class="tiktok-action-button"><span class="material-icons">chat_bubble_outline</span>34K</button>
                    <button class="tiktok-action-button"><span class="material-icons">share</span>12K</button>
                    <button class="tiktok-action-button"><span class="material-icons">card_giftcard</span>Gift</button>
                </div>
            </div>
            <div class="tiktok-video-container">
                <video class="tiktok-video-player" src="https://placehold.co/1080x1920.mp4?text=Video+2" loop muted playsinline></video>
                 <div class="tiktok-video-overlay">
                    <div class="tiktok-video-info">
                        <h3>Amazing Dance Moves</h3>
                        <p>Uploaded by <span class="uploader-name">DanceStar</span></p>
                    </div>
                </div>
                <div class="tiktok-video-actions">
                    <button class="tiktok-action-button"><span class="material-icons">favorite</span>890K</button>
                    <button class="tiktok-action-button"><span class="material-icons">chat_bubble_outline</span>15K</button>
                    <button class="tiktok-action-button"><span class="material-icons">share</span>5K</button>
                    <button class="tiktok-action-button"><span class="material-icons">card_giftcard</span>Gift</button>
                </div>
            </div>
        </section>

        <section id="movies-section" class="main-section hidden p-4 md:p-8">
            <h2 class="text-3xl font-bold mb-6">Movies</h2>
            <div class="movie-player-container mb-8">
                <video class="movie-player" controls poster="https://placehold.co/1280x720/000000/FFFFFF?text=Movie+Poster">
                    <source src="https://placehold.co/1280x720.mp4?text=Movie+Playback" type="video/mp4">
                    Your browser does not support the video tag.
                </video>
            </div>
            <div class="movie-details">
                <h3 class="movie-title">Epic Adventure in the Cosmos</h3>
                <p class="movie-meta-info">2h 15m | Action, Sci-Fi | PG-13 | 2.1M Views | Uploaded 2 weeks ago</p>
                <p class="movie-description">
                    Join a band of unlikely heroes as they travel across galaxies to save the universe from an ancient evil.
                    Packed with stunning visuals and heart-pounding action.
                </p>
                <div class="movie-interactions">
                    <button class="movie-interaction-button"><span class="material-icons">thumb_up</span> 150K</button>
                    <button class="movie-interaction-button"><span class="material-icons">thumb_down</span> 2K</button>
                    <button class="movie-interaction-button"><span class="material-icons">share</span> Share</button>
                    <button class="movie-interaction-button"><span class="material-icons">playlist_add</span> Save</button>
                    <button class="movie-interaction-button"><span class="material-icons">download</span> Download</button>
                </div>
            </div>
            <div class="movie-comments-section">
                <h4 class="text-xl font-semibold mb-4 text-white">Comments (3)</h4>
                <div class="movie-comment">
                    <strong>User123:</strong> <p>Amazing movie! Best sci-fi I've seen this year.</p>
                </div>
                <div class="movie-comment">
                    <strong>MovieFanatic:</strong> <p>The special effects were incredible. Highly recommend!</p>
                </div>
                <form class="movie-comment-form">
                    <input type="text" placeholder="Add a comment..." class="form-input">
                    <button type="submit">Comment</button>
                </form>
            </div>
        </section>

        <section id="music-section" class="main-section hidden p-4 md:p-8">
            <h2 class="text-3xl font-bold mb-6">Music</h2>
            <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-6">
                <div class="music-card">
                    <img src="https://placehold.co/300x300/8b5cf6/ffffff?text=Album+Art+1" alt="Album Art" class="music-card-thumbnail">
                    <div class="music-card-content">
                        <h3 class="music-card-title">Midnight Drive</h3>
                        <p class="music-card-artist">Synthwave Dreams</p>
                        <div class="music-card-meta">
                            <span>Album: Retro Futures</span>
                            <span class="material-icons download-icon" title="Download Track">download</span>
                        </div>
                    </div>
                </div>
                <div class="music-card">
                    <img src="https://placehold.co/300x300/10b981/ffffff?text=Album+Art+2" alt="Album Art" class="music-card-thumbnail">
                    <div class="music-card-content">
                        <h3 class="music-card-title">Ocean Breeze</h3>
                        <p class="music-card-artist">Chill Vibes Only</p>
                         <div class="music-card-meta">
                            <span>Single</span>
                            <span class="material-icons download-icon" title="Download Track">download</span>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="live-streams-section" class="main-section hidden p-4 md:p-8">
            <h2 class="text-3xl font-bold mb-6">Live Streams</h2>
            <p class="text-gray-400">No live streams currently. Check back later!</p>
        </section>

        <section id="profile-section" class="main-section hidden p-4 md:p-8">
            <div class="profile-header">
                <img id="profile-page-avatar" src="https://placehold.co/100x100/7c3aed/ffffff?text=A" alt="User Avatar" class="profile-avatar">
                <div class="profile-info">
                    <h2 id="profile-page-name">Alice Smith <span id="profile-page-verified-badge" class="hidden material-icons verified-badge">verified</span></h2>
                    <p id="profile-page-bio" class="text-gray-400">Loves creating content and sharing with the world! 🌍✨</p>
                    <div class="profile-stats">
                        <span><strong id="profile-followers">1.2M</strong> Followers</span>
                        <span><strong id="profile-following">345</strong> Following</span>
                        <span><strong id="profile-uploads">78</strong> Uploads</span>
                    </div>
                </div>
            </div>

            <div class="profile-tabs">
                <button class="profile-tab-button active" data-profile-tab="my-videos">My Videos</button>
                <button class="profile-tab-button" data-profile-tab="liked">Liked</button>
                <button class="profile-tab-button" data-profile-tab="wallet">Wallet</button>
                <button class="profile-tab-button" data-profile-tab="settings">Settings</button>
            </div>

            <div id="profile-content-my-videos" class="profile-content-section active">
                <h3 class="text-xl font-semibold mb-4">My Videos</h3>
                <p class="text-gray-400">You haven't uploaded any videos yet.</p>
            </div>
            <div id="profile-content-liked" class="profile-content-section">
                <h3 class="text-xl font-semibold mb-4">Liked Content</h3>
                <p class="text-gray-400">You haven't liked any content yet.</p>
            </div>
            <div id="profile-content-wallet" class="profile-content-section wallet-section">
                <div class="wallet-header">
                    <h3>My Wallet</h3>
                    <div class="wallet-actions">
                        <button id="deposit-button-profile" class="btn btn-primary"><span class="material-icons text-lg mr-1">add_card</span> Deposit</button>
                        <button id="withdraw-button-profile" class="btn btn-secondary"><span class="material-icons text-lg mr-1">payments</span> Withdraw</button>
                    </div>
                </div>
                <div class="wallet-balance-container">
                    <p class="wallet-balance-label">Available Balance</p>
                    <p class="wallet-balance" id="profile-wallet-balance">$123.45</p>
                </div>
                <h4 class="text-lg font-medium text-white mb-2">Transaction History</h4>
                <p class="text-gray-400 text-sm">No transactions yet.</p>
            </div>
            <div id="profile-content-settings" class="profile-content-section">
                <h3 class="text-xl font-semibold mb-4">Account Settings</h3>
                <form id="profile-settings-form">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div class="form-group">
                            <label for="profile-setting-name" class="form-label">Display Name</label>
                            <input type="text" id="profile-setting-name" class="form-input" value="Alice Smith">
                        </div>
                        <div class="form-group">
                            <label for="profile-setting-email" class="form-label">Email Address</label>
                            <input type="email" id="profile-setting-email" class="form-input" value="alice@example.com" readonly>
                             <p class="text-xs text-gray-500 mt-1">Email cannot be changed here.</p>
                        </div>
                        <div class="form-group md:col-span-2">
                            <label for="profile-setting-bio" class="form-label">Bio</label>
                            <textarea id="profile-setting-bio" class="form-textarea" rows="3">Loves creating content and sharing with the world! 🌍✨</textarea>
                        </div>
                        <div class="form-group">
                            <label for="profile-setting-avatar" class="form-label">Profile Picture</label>
                            <input type="file" id="profile-setting-avatar" class="form-input" accept="image/*">
                            <img src="https://placehold.co/100x100/7c3aed/ffffff?text=A" alt="Avatar Preview" id="profile-avatar-preview" class="image-preview mt-2">
                        </div>
                        <div class="form-group">
                            <label for="profile-setting-banner" class="form-label">Profile Banner</label>
                            <input type="file" id="profile-setting-banner" class="form-input" accept="image/*">
                            <img src="https://placehold.co/600x150/4a5568/ffffff?text=Banner+Preview" alt="Banner Preview" id="profile-banner-preview" class="banner-preview mt-2">
                        </div>
                    </div>
                    <div class="form-buttons mt-6">
                        <button type="submit" class="form-button form-button-submit">Save Changes</button>
                    </div>
                </form>
            </div>
        </section>

        <section id="live-studio-section" class="main-section hidden p-4 md:p-8">
            <div class="flex flex-col lg:flex-row gap-8">
                <div class="lg:w-2/3">
                    <h2 class="text-3xl font-bold mb-2">Live Studio</h2>
                    <div class="flex items-center gap-2 mb-4">
                        <span id="live-status" class="live-status-indicator offline">Offline</span>
                        <span id="live-viewers" class="text-sm text-gray-400 hidden"><span class="material-icons text-base mr-1">visibility</span> <span id="viewer-count-live">0</span> viewers</span>
                    </div>

                    <video id="live-video-preview" class="live-studio-video-preview" autoplay muted playsinline></video>
                    
                    <div class="live-studio-controls space-y-4 mb-6">
                        <div>
                            <label for="stream-title" class="form-label">Stream Title</label>
                            <input type="text" id="stream-title" class="form-input" placeholder="My Awesome Live Stream">
                        </div>
                        <div>
                            <label for="stream-description" class="form-label">Description</label>
                            <textarea id="stream-description" class="form-textarea" rows="3" placeholder="Talking about cool stuff today!"></textarea>
                        </div>
                        <div class="flex flex-col sm:flex-row gap-4">
                             <button id="start-stream-button" class="live-studio-button btn-go-live w-full sm:w-auto justify-center">
                                <span class="material-icons">sensors</span> Go Live
                            </button>
                            <button id="stop-stream-button" class="live-studio-button btn-stop-stream w-full sm:w-auto justify-center hidden">
                                <span class="material-icons">stop_circle</span> Stop Stream
                            </button>
                             <button id="mic-toggle-button" class="live-studio-button btn-secondary w-full sm:w-auto justify-center">
                                <span class="material-icons" id="mic-icon">mic</span> <span id="mic-text">Mute</span>
                            </button>
                        </div>
                        <p id="live-studio-error" class="text-red-400 text-sm hidden"></p>
                    </div>
                </div>

                <div class="lg:w-1/3">
                    <h3 class="text-xl font-semibold mb-4">Live Chat</h3>
                    <div class="bg-gray-800/50 border border-white/10 rounded-md p-4 h-96 overflow-y-auto flex flex-col">
                        <div id="chat-messages-live" class="flex-grow space-y-2">
                            <p class="live-studio-placeholder-text text-sm">Chat messages will appear here...</p>
                        </div>
                        <div class="mt-4 flex gap-2">
                            <input type="text" id="chat-input-live" class="form-input flex-grow bg-gray-700/80" placeholder="Say something...">
                            <button id="send-chat-button-live" class="btn btn-primary">Send</button>
                        </div>
                    </div>
                </div>
            </div>
        </section>


    </main>

    <footer class="bg-black/50 border-t border-white/10 py-6 text-center mt-auto">
        <p class="text-sm text-gray-400">&copy; <span id="current-year"></span> Streamo. All rights reserved.</p>
        <p class="text-xs text-gray-500 mt-1">Your Ultimate Streaming Destination.</p>
    </footer>

    <div id="signin-modal" class="modal-overlay">
        <div class="modal-content">
            <button class="modal-close-button" data-modal-close="signin-modal">&times;</button>
            <div class="modal-header">
                <h2 class="modal-title">Sign In to Streamo</h2>
                <p class="modal-description">Access your account and continue streaming.</p>
            </div>
            <form id="signin-form">
                <div class="form-group">
                    <label for="signin-email" class="form-label">Email Address</label>
                    <input type="email" id="signin-email" class="form-input" placeholder="you@example.com" required>
                </div>
                <div class="form-group">
                    <label for="signin-password" class="form-label">Password</label>
                    <input type="password" id="signin-password" class="form-input" placeholder="••••••••" required>
                </div>
                <p id="signin-error" class="form-error hidden"><span class="material-icons">error_outline</span><span></span></p>
                <div class="form-buttons">
                    <button type="button" class="form-button form-button-cancel" data-modal-close="signin-modal">Cancel</button>
                    <button type="submit" class="form-button form-button-submit">Sign In</button>
                </div>
            </form>
            <div class="text-center my-4 text-gray-400 text-sm">OR CONTINUE WITH</div>
            <div class="space-y-3">
                 <button class="btn-social btn-google" id="google-signin-button">
                    <svg viewBox="0 0 48 48"><path fill="#EA4335" d="M24 9.5c3.54 0 6.71 1.22 9.21 3.6l6.85-6.85C35.9 2.38 30.47 0 24 0 14.62 0 6.51 5.38 2.56 13.22l7.98 6.19C12.43 13.72 17.74 9.5 24 9.5z"></path><path fill="#4285F4" d="M46.98 24.55c0-1.57-.15-3.09-.38-4.55H24v9.02h12.94c-.58 2.96-2.26 5.48-4.78 7.18l7.73 6c4.51-4.18 7.09-10.36 7.09-17.65z"></path><path fill="#FBBC05" d="M10.53 28.59c-.48-1.45-.76-2.99-.76-4.59s.27-3.14.76-4.59l-7.98-6.19C.92 16.46 0 20.12 0 24c0 3.88.92 7.54 2.56 10.78l7.97-6.19z"></path><path fill="#34A853" d="M24 48c6.48 0 11.93-2.13 15.89-5.81l-7.73-6c-2.15 1.45-4.92 2.3-8.16 2.3-6.26 0-11.57-4.22-13.47-9.91l-7.98 6.19C6.51 42.62 14.62 48 24 48z"></path><path fill="none" d="M0 0h48v48H0z"></path></svg>
                    Sign in with Google
                </button>
                <button class="btn-social btn-facebook" id="facebook-signin-button">
                    <span class="material-icons">facebook</span>
                    Sign in with Facebook
                </button>
                <button class="btn-social btn-tiktok" id="tiktok-signin-button">
                     <svg fill="currentColor" viewBox="0 0 24 24" class="w-5 h-5"><path d="M21.0528 7.05762C20.0005 7.03062 18.9401 7.00362 17.8878 6.99952V14.5005C17.8878 18.0535 14.9907 20.9895 11.4938 20.9895C8.00091 20.9895 5.11279 18.0535 5.11279 14.5005C5.11279 10.9475 8.00091 8.01152 11.4938 8.01152C12.0737 8.01152 12.6416 8.08952 13.1826 8.22652V11.6735C13.1597 11.6575 13.1288 11.6495 13.102 11.6415C13.0342 11.6175 12.9664 11.5935 12.8945 11.5735C12.8706 11.5655 12.8466 11.5535 12.8227 11.5455C12.6151 11.4895 12.4035 11.4495 12.188 11.4255C12.172 11.4215 12.1561 11.4215 12.1441 11.4175C11.9446 11.3935 11.7331 11.3775 11.5256 11.3775C9.88691 11.3775 8.53219 12.7795 8.53219 14.4295C8.53219 16.0795 9.88691 17.4855 11.5256 17.4855C13.1644 17.4855 14.5191 16.0795 14.5191 14.4295V6.08662C14.5191 4.27062 15.3089 2.57362 16.6368 1.46162C16.9569 1.19762 17.321 0.978621 17.7162 0.810621C17.7321 0.802621 17.7521 0.794621 17.7681 0.786621C18.4163 0.526621 19.1086 0.343621 19.832 0.250621L19.896 0.242621C20.3012 0.190621 20.7105 0.162621 21.1238 0.158621L21.5008 0.150621C21.4618 0.138621 21.4269 0.122621 21.3879 0.110621C21.3799 0.106621 21.3759 0.106621 21.3679 0.102621C21.3319 0.0906212 21.296 0.0786212 21.2601 0.0666212C21.2281 0.0546212 21.1922 0.0466212 21.1602 0.0346212C20.6273 -0.0773788 20.0745 -0.00537882 19.6139 0.0306212C19.1492 0.0666212 18.6926 0.102621 18.2441 0.158621L18.2121 0.162621C17.5639 0.242621 16.9316 0.406621 16.3324 0.642621C16.3044 0.654621 16.2805 0.670621 16.2524 0.682621C14.6037 1.39462 13.1826 2.91062 13.1826 4.95462V7.05762H21.0528Z"></path></svg>
                    Sign in with TikTok
                </button>
                 <button class="btn-social btn-x" id="x-signin-button">
                    <svg fill="currentColor" viewBox="0 0 24 24" class="w-5 h-5"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"></path></svg>
                    Sign in with X
                </button>
            </div>
        </div>
    </div>

    <div id="signup-modal" class="modal-overlay">
         <div class="modal-content">
            <button class="modal-close-button" data-modal-close="signup-modal">&times;</button>
            <div class="modal-header">
                <h2 class="modal-title">Create Account</h2>
                <p class="modal-description">Join Streamo today and start exploring!</p>
            </div>
            <form id="signup-form">
                <div class="form-group">
                    <label for="signup-username" class="form-label">Username</label>
                    <input type="text" id="signup-username" class="form-input" placeholder="yourusername" required>
                </div>
                <div class="form-group">
                    <label for="signup-email" class="form-label">Email Address</label>
                    <input type="email" id="signup-email" class="form-input" placeholder="you@example.com" required>
                </div>
                <div class="form-group">
                    <label for="signup-password" class="form-label">Password</label>
                    <input type="password" id="signup-password" class="form-input" placeholder="••••••••" required>
                </div>
                 <p id="signup-error" class="form-error hidden"><span class="material-icons">error_outline</span><span></span></p>
                <div class="form-buttons">
                    <button type="button" class="form-button form-button-cancel" data-modal-close="signup-modal">Cancel</button>
                    <button type="submit" class="form-button form-button-submit">Sign Up</button>
                </div>
            </form>
            <div class="text-center my-4 text-gray-400 text-sm">OR SIGN UP WITH</div>
            <div class="space-y-3">
                 <button class="btn-social btn-google" id="google-signup-button">
                     <svg viewBox="0 0 48 48"><path fill="#EA4335" d="M24 9.5c3.54 0 6.71 1.22 9.21 3.6l6.85-6.85C35.9 2.38 30.47 0 24 0 14.62 0 6.51 5.38 2.56 13.22l7.98 6.19C12.43 13.72 17.74 9.5 24 9.5z"></path><path fill="#4285F4" d="M46.98 24.55c0-1.57-.15-3.09-.38-4.55H24v9.02h12.94c-.58 2.96-2.26 5.48-4.78 7.18l7.73 6c4.51-4.18 7.09-10.36 7.09-17.65z"></path><path fill="#FBBC05" d="M10.53 28.59c-.48-1.45-.76-2.99-.76-4.59s.27-3.14.76-4.59l-7.98-6.19C.92 16.46 0 20.12 0 24c0 3.88.92 7.54 2.56 10.78l7.97-6.19z"></path><path fill="#34A853" d="M24 48c6.48 0 11.93-2.13 15.89-5.81l-7.73-6c-2.15 1.45-4.92 2.3-8.16 2.3-6.26 0-11.57-4.22-13.47-9.91l-7.98 6.19C6.51 42.62 14.62 48 24 48z"></path><path fill="none" d="M0 0h48v48H0z"></path></svg>
                    Sign up with Google
                </button>
                <button class="btn-social btn-facebook" id="facebook-signup-button">
                    <span class="material-icons">facebook</span>
                    Sign up with Facebook
                </button>
                <button class="btn-social btn-tiktok" id="tiktok-signup-button">
                    <svg fill="currentColor" viewBox="0 0 24 24" class="w-5 h-5"><path d="M21.0528 7.05762C20.0005 7.03062 18.9401 7.00362 17.8878 6.99952V14.5005C17.8878 18.0535 14.9907 20.9895 11.4938 20.9895C8.00091 20.9895 5.11279 18.0535 5.11279 14.5005C5.11279 10.9475 8.00091 8.01152 11.4938 8.01152C12.0737 8.01152 12.6416 8.08952 13.1826 8.22652V11.6735C13.1597 11.6575 13.1288 11.6495 13.102 11.6415C13.0342 11.6175 12.9664 11.5935 12.8945 11.5735C12.8706 11.5655 12.8466 11.5535 12.8227 11.5455C12.6151 11.4895 12.4035 11.4495 12.188 11.4255C12.172 11.4215 12.1561 11.4215 12.1441 11.4175C11.9446 11.3935 11.7331 11.3775 11.5256 11.3775C9.88691 11.3775 8.53219 12.7795 8.53219 14.4295C8.53219 16.0795 9.88691 17.4855 11.5256 17.4855C13.1644 17.4855 14.5191 16.0795 14.5191 14.4295V6.08662C14.5191 4.27062 15.3089 2.57362 16.6368 1.46162C16.9569 1.19762 17.321 0.978621 17.7162 0.810621C17.7321 0.802621 17.7521 0.794621 17.7681 0.786621C18.4163 0.526621 19.1086 0.343621 19.832 0.250621L19.896 0.242621C20.3012 0.190621 20.7105 0.162621 21.1238 0.158621L21.5008 0.150621C21.4618 0.138621 21.4269 0.122621 21.3879 0.110621C21.3799 0.106621 21.3759 0.106621 21.3679 0.102621C21.3319 0.0906212 21.296 0.0786212 21.2601 0.0666212C21.2281 0.0546212 21.1922 0.0466212 21.1602 0.0346212C20.6273 -0.0773788 20.0745 -0.00537882 19.6139 0.0306212C19.1492 0.0666212 18.6926 0.102621 18.2441 0.158621L18.2121 0.162621C17.5639 0.242621 16.9316 0.406621 16.3324 0.642621C16.3044 0.654621 16.2805 0.670621 16.2524 0.682621C14.6037 1.39462 13.1826 2.91062 13.1826 4.95462V7.05762H21.0528Z"></path></svg>
                    Sign up with TikTok
                </button>
                 <button class="btn-social btn-x" id="x-signup-button">
                    <svg fill="currentColor" viewBox="0 0 24 24" class="w-5 h-5"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"></path></svg>
                    Sign up with X
                </button>
            </div>
        </div>
    </div>

    <div id="upload-modal" class="modal-overlay">
        <div class="modal-content">
            <button class="modal-close-button" data-modal-close="upload-modal">&times;</button>
            <div class="modal-header">
                <h2 class="modal-title">Upload Content</h2>
                <p class="modal-description">Share your videos, music, or start a live stream.</p>
            </div>
            <form id="upload-form">
                <div class="form-group">
                    <label for="upload-title" class="form-label">Title</label>
                    <input type="text" id="upload-title" class="form-input" placeholder="My Awesome Content" required>
                </div>
                <div class="form-group">
                    <label for="upload-description" class="form-label">Description</label>
                    <textarea id="upload-description" class="form-textarea" rows="3" placeholder="Tell us about your content..."></textarea>
                </div>
                <div class="form-group">
                    <label for="upload-type" class="form-label">Content Type</label>
                    <select id="upload-type" class="form-select">
                        <option value="video">Video</option>
                        <option value="music">Music Track</option>
                        <option value="movie">Movie</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="upload-file" class="form-label">File</label>
                    <input type="file" id="upload-file" class="form-input">
                    <p class="file-info">Max file size: 500MB. Supported formats: MP4, MOV, MP3, WAV.</p>
                </div>
                <div class="form-group">
                    <label for="upload-thumbnail" class="form-label">Thumbnail (Optional)</label>
                    <input type="file" id="upload-thumbnail-input" class="form-input" accept="image/*">
                    <img src="https://placehold.co/100x100/4a5568/ffffff?text=Preview" alt="Thumbnail Preview" id="upload-thumbnail-preview" class="image-preview mt-2">
                     <p id="video-watermark-info" class="watermark-info hidden">Uploaded videos will be processed to include the Streamo logo.</p>
                </div>
                 <p id="upload-error" class="form-error hidden"><span class="material-icons">error_outline</span><span></span></p>
                 <p id="upload-success" class="form-success hidden"><span class="material-icons">check_circle_outline</span><span></span></p>
                <div class="form-buttons">
                    <button type="button" class="form-button form-button-cancel" data-modal-close="upload-modal">Cancel</button>
                    <button type="submit" class="form-button form-button-submit">
                        <span class="material-icons mr-1 text-lg">cloud_upload</span> Upload
                    </button>
                </div>
            </form>
        </div>
    </div>
    
    <div id="music-now-playing-bar" class="music-now-playing-bar hidden">
        <div class="flex items-center gap-3">
            <img id="now-playing-art" src="https://placehold.co/40x40/8b5cf6/ffffff?text=S" alt="Album Art">
            <div>
                <p id="now-playing-title" class="text-sm font-medium text-white">Song Title</p>
                <p id="now-playing-artist" class="text-xs text-gray-400">Artist Name</p>
            </div>
        </div>
        <div class="music-controls flex items-center gap-2 text-white">
            <button title="Previous"><span class="material-icons">skip_previous</span></button>
            <button title="Play/Pause"><span class="material-icons text-3xl">play_circle_filled</span></button>
            <button title="Next"><span class="material-icons">skip_next</span></button>
        </div>
        <div class="flex items-center gap-2 text-white">
            <span class="material-icons text-lg">volume_up</span>
        </div>
    </div>


<script>
    document.addEventListener('DOMContentLoaded', () => {
        // --- Constants and DOM Elements ---
        const navLinks = document.querySelectorAll('.nav-link, .mobile-menu-link.active ~ .mobile-menu-link, .mobile-menu-link:not(.active ~ .mobile-menu-link)');
        const mobileMenuLinks = document.querySelectorAll('.mobile-menu-link');
        const mainSections = document.querySelectorAll('.main-section');
        const mobileMenuOpenButton = document.getElementById('mobile-menu-open-button');
        const mobileMenuCloseButton = document.getElementById('mobile-menu-close-button');
        const mobileMenuContainer = document.getElementById('mobile-menu-container');
        const currentYearSpan = document.getElementById('current-year');

        const authButtonsDesktop = document.getElementById('auth-buttons-desktop');
        const userAreaDesktop = document.getElementById('user-area-desktop');
        const authButtonsMobile = document.getElementById('auth-buttons-mobile');
        const userAreaMobile = document.getElementById('user-area-mobile'); 

        const userDropdownButton = document.getElementById('user-dropdown-button');
        const dropdownMenu = document.getElementById('dropdown-menu');
        const profileTabChangers = document.querySelectorAll('.profile-tab-changer');

        const signinButtonDesktop = document.getElementById('signin-button-desktop');
        const signupButtonDesktop = document.getElementById('signup-button-desktop');
        const signinButtonMobile = document.getElementById('signin-button-mobile');
        const signupButtonMobile = document.getElementById('signup-button-mobile');
        const uploadButton = document.getElementById('upload-button');
        const uploadButtonMobile = document.getElementById('upload-button-mobile');

        const signinModal = document.getElementById('signin-modal');
        const signupModal = document.getElementById('signup-modal');
        const uploadModal = document.getElementById('upload-modal');
        const modals = [signinModal, signupModal, uploadModal];
        const modalCloseButtons = document.querySelectorAll('.modal-close-button, .form-button-cancel');

        const googleSigninButton = document.getElementById('google-signin-button');
        const facebookSigninButton = document.getElementById('facebook-signin-button');
        const tiktokSigninButton = document.getElementById('tiktok-signin-button');
        const xSigninButton = document.getElementById('x-signin-button');
        const googleSignupButton = document.getElementById('google-signup-button');
        const facebookSignupButton = document.getElementById('facebook-signup-button');
        const tiktokSignupButton = document.getElementById('tiktok-signup-button');
        const xSignupButton = document.getElementById('x-signup-button');

        const profileTabs = document.querySelectorAll('.profile-tab-button');
        const profileContentSections = document.querySelectorAll('.profile-content-section');
        const profileAvatarInput = document.getElementById('profile-setting-avatar');
        const profileAvatarPreview = document.getElementById('profile-avatar-preview');
        const profileBannerInput = document.getElementById('profile-setting-banner');
        const profileBannerPreview = document.getElementById('profile-banner-preview');
        
        const uploadFileInput = document.getElementById('upload-file'); // For main file
        const uploadThumbnailInput = document.getElementById('upload-thumbnail-input'); // For custom thumbnail
        const uploadThumbnailPreview = document.getElementById('upload-thumbnail-preview');
        const videoWatermarkInfo = document.getElementById('video-watermark-info');


        const tiktokVideosSection = document.getElementById('tiktok-videos-section');
        let currentPlayingTikTokVideo = null;

        const liveVideoPreview = document.getElementById('live-video-preview');
        const startStreamButton = document.getElementById('start-stream-button');
        const stopStreamButton = document.getElementById('stop-stream-button');
        const micToggleButton = document.getElementById('mic-toggle-button');
        const micIcon = document.getElementById('mic-icon');
        const micText = document.getElementById('mic-text');
        const liveStatusIndicator = document.getElementById('live-status');
        const liveViewersCount = document.getElementById('live-viewers');
        const liveStudioError = document.getElementById('live-studio-error');
        let localStream = null;
        let isMicMuted = false;

        // Streamo Logo SVG (simple text version)
        const streamoLogoSVG = `<svg xmlns="http://www.w3.org/2000/svg" width="100" height="25" viewBox="0 0 100 25">
            <rect width="100" height="25" fill="rgba(0,0,0,0.4)" rx="5"/>
            <text x="50" y="17" font-family="Inter, sans-serif" font-size="14" fill="rgba(255,255,255,0.8)" text-anchor="middle" font-weight="bold">Streamo</text>
        </svg>`;
        const streamoLogoImage = new Image();
        streamoLogoImage.src = 'data:image/svg+xml;base64,' + btoa(streamoLogoSVG);


        // --- State ---
        let isLoggedIn = false; 
        let currentUser = {
            name: "Alice Smith", 
            email: "alice@example.com",
            avatar: "https://placehold.co/40x40/7c3aed/ffffff?text=A",
            isVerified: true,
            bio: "Loves creating content and sharing with the world! 🌍✨",
            followers: "1.2M",
            following: "345",
            uploads: "78",
            walletBalance: "$123.45"
        };

        // --- Initialization ---
        if (currentYearSpan) {
            currentYearSpan.textContent = new Date().getFullYear();
        }
        updateAuthStateUI();
        setActiveSection('tiktok-videos-section'); 

        // --- Functions ---

        function stopCurrentCameraStream() {
            if (localStream && liveVideoPreview) { 
                localStream.getTracks().forEach(track => track.stop());
                localStream = null;
                liveVideoPreview.srcObject = null;
                if(liveStatusIndicator) liveStatusIndicator.textContent = 'Offline';
                if(liveStatusIndicator) liveStatusIndicator.classList.remove('live');
                if(liveStatusIndicator) liveStatusIndicator.classList.add('offline');
                if(startStreamButton) startStreamButton.classList.remove('hidden');
                if(stopStreamButton) stopStreamButton.classList.add('hidden');
                if(liveViewersCount) liveViewersCount.classList.add('hidden');
                if(liveStudioError) liveStudioError.classList.add('hidden');
                console.log("Camera stream stopped for Live Studio.");
            }
        }
        
        function setActiveSection(targetSectionId, targetProfileTab = null) {
            const liveStudioSec = document.getElementById('live-studio-section');
            if (liveStudioSec && !liveStudioSec.classList.contains('hidden') && targetSectionId !== 'live-studio-section') {
                stopCurrentCameraStream();
            }

            mainSections.forEach(section => {
                section.classList.add('hidden');
                if (section.id === targetSectionId) {
                    section.classList.remove('hidden');
                }
            });

            navLinks.forEach(link => {
                link.classList.remove('active');
                if (link.dataset.section === targetSectionId) {
                    link.classList.add('active');
                }
            });
            mobileMenuLinks.forEach(link => {
                link.classList.remove('active');
                 if (link.dataset.section === targetSectionId) {
                    link.classList.add('active');
                }
            });

            if (targetSectionId === 'profile-section' && targetProfileTab) {
                setActiveProfileTab(targetProfileTab);
            }

            if (targetSectionId === 'tiktok-videos-section') {
                adjustTikTokSectionHeight();
                initializeTikTokPlayer();
            } else {
                if (currentPlayingTikTokVideo) {
                    currentPlayingTikTokVideo.pause();
                }
            }
            
            if (targetSectionId === 'live-studio-section' && !localStream) {
                // Live studio camera is started by user click
            }

            window.scrollTo(0, 0); 
            closeMobileMenu();
        }

        function openMobileMenu() {
            if (mobileMenuContainer) mobileMenuContainer.classList.add('open');
        }
        function closeMobileMenu() {
            if (mobileMenuContainer) mobileMenuContainer.classList.remove('open');
        }

        function updateAuthStateUI() {
            const userAvatarNav = document.getElementById('user-avatar-nav');
            const userNameNav = document.getElementById('user-name-nav');
            const dropdownUserName = document.getElementById('dropdown-user-name');
            const dropdownUserEmail = document.getElementById('dropdown-user-email');
            const userVerifiedBadgeNav = document.getElementById('user-verified-badge-nav');
            const dropdownUserVerifiedBadge = document.getElementById('dropdown-user-verified-badge');

            if (isLoggedIn) {
                if (authButtonsDesktop) authButtonsDesktop.classList.add('hidden');
                if (userAreaDesktop) userAreaDesktop.classList.remove('hidden');
                if (authButtonsMobile) authButtonsMobile.classList.add('hidden');
                if (userAreaMobile) userAreaMobile.classList.remove('hidden');

                if(userAvatarNav) userAvatarNav.src = currentUser.avatar;
                if(userNameNav) userNameNav.textContent = currentUser.name;
                if(dropdownUserName) dropdownUserName.textContent = currentUser.name;
                if(dropdownUserEmail) dropdownUserEmail.textContent = currentUser.email;
                
                if (currentUser.isVerified) {
                    if(userVerifiedBadgeNav) userVerifiedBadgeNav.classList.remove('hidden');
                    if(dropdownUserVerifiedBadge) dropdownUserVerifiedBadge.classList.remove('hidden');
                } else {
                    if(userVerifiedBadgeNav) userVerifiedBadgeNav.classList.add('hidden');
                    if(dropdownUserVerifiedBadge) dropdownUserVerifiedBadge.classList.add('hidden');
                }

                const profilePageAvatar = document.getElementById('profile-page-avatar');
                const profilePageName = document.getElementById('profile-page-name');
                const profilePageBio = document.getElementById('profile-page-bio');
                const profileFollowers = document.getElementById('profile-followers');
                const profileFollowing = document.getElementById('profile-following');
                const profileUploads = document.getElementById('profile-uploads');
                const profileWalletBalance = document.getElementById('profile-wallet-balance');
                const profileSettingName = document.getElementById('profile-setting-name');
                const profileSettingEmail = document.getElementById('profile-setting-email');
                const profileSettingBio = document.getElementById('profile-setting-bio');
                const profileAvatarPrev = document.getElementById('profile-avatar-preview'); 
                const profilePageVerifiedBadge = document.getElementById('profile-page-verified-badge');


                if (profilePageAvatar) profilePageAvatar.src = currentUser.avatar;
                if (profilePageName) profilePageName.childNodes[0].nodeValue = currentUser.name + ' '; 
                if (profilePageBio) profilePageBio.textContent = currentUser.bio;
                if (profileFollowers) profileFollowers.textContent = currentUser.followers;
                if (profileFollowing) profileFollowing.textContent = currentUser.following;
                if (profileUploads) profileUploads.textContent = currentUser.uploads;
                if (profileWalletBalance) profileWalletBalance.textContent = currentUser.walletBalance;
                if (profileSettingName) profileSettingName.value = currentUser.name;
                if (profileSettingEmail) profileSettingEmail.value = currentUser.email;
                if (profileSettingBio) profileSettingBio.value = currentUser.bio;
                if (profileAvatarPrev) profileAvatarPrev.src = currentUser.avatar; 

                if (profilePageVerifiedBadge) {
                    if (currentUser.isVerified) profilePageVerifiedBadge.classList.remove('hidden');
                    else profilePageVerifiedBadge.classList.add('hidden');
                }


            } else {
                if (authButtonsDesktop) authButtonsDesktop.classList.remove('hidden');
                if (userAreaDesktop) userAreaDesktop.classList.add('hidden');
                if (authButtonsMobile) authButtonsMobile.classList.remove('hidden');
                if (userAreaMobile) userAreaMobile.classList.add('hidden');
            }
        }

        function toggleUserDropdown() {
            if (dropdownMenu) dropdownMenu.classList.toggle('hidden');
        }

        function openModal(modalElement) {
            if (modalElement) {
                 modals.forEach(m => { if(m) m.classList.remove('open'); }); 
                 modalElement.classList.add('open');
            }
        }
        function closeModal(modalElement) {
            if (modalElement) modalElement.classList.remove('open');
        }
        function closeAllModals() {
            modals.forEach(modal => {if(modal) closeModal(modal);});
        }

        function setActiveProfileTab(tabId) {
            profileTabs.forEach(tab => {
                tab.classList.remove('active');
                if (tab.dataset.profileTab === tabId) {
                    tab.classList.add('active');
                }
            });
            profileContentSections.forEach(section => {
                section.classList.remove('active');
                if (section.id === `profile-content-${tabId}`) {
                    section.classList.add('active');
                }
            });
        }
        
        // Generic image previewer for file inputs
        function setupImagePreview(fileInputElement, previewElement) {
            if (fileInputElement && previewElement) {
                // Store default src for reset if not already stored
                if (!previewElement.dataset.defaultSrc) {
                    previewElement.dataset.defaultSrc = previewElement.src;
                }

                fileInputElement.addEventListener('change', function(event) {
                    const file = event.target.files[0];
                    if (file && file.type.startsWith('image/')) {
                        const reader = new FileReader();
                        reader.onload = function(e) {
                            previewElement.src = e.target.result;
                        }
                        reader.readAsDataURL(file);
                    } else {
                        // Reset to placeholder if no file or non-image file is selected
                        previewElement.src = previewElement.dataset.defaultSrc;
                    }
                });
            }
        }
        
        // Function to generate and display a watermarked thumbnail for videos
        function generateWatermarkedVideoThumbnail(videoFile, thumbnailPreviewEl) {
            if (!videoFile || !videoFile.type.startsWith('video/')) {
                if (videoWatermarkInfo) videoWatermarkInfo.classList.add('hidden');
                // If not a video, try to show regular image preview if an image was selected for thumbnail
                const customThumbFile = uploadThumbnailInput.files[0];
                if (customThumbFile && customThumbFile.type.startsWith('image/')) {
                     const reader = new FileReader();
                     reader.onload = (e) => thumbnailPreviewEl.src = e.target.result;
                     reader.readAsDataURL(customThumbFile);
                } else {
                    thumbnailPreviewEl.src = thumbnailPreviewEl.dataset.defaultSrc; // Reset if no valid image
                }
                return;
            }

            if (videoWatermarkInfo) videoWatermarkInfo.classList.remove('hidden');

            const reader = new FileReader();
            reader.onload = (e) => {
                const video = document.createElement('video');
                video.preload = 'metadata';
                video.onloadeddata = () => {
                    window.URL.revokeObjectURL(video.src); // Clean up object URL
                    const canvas = document.createElement('canvas');
                    canvas.width = video.videoWidth;
                    canvas.height = video.videoHeight;
                    const ctx = canvas.getContext('2d');
                    
                    // Draw video frame
                    ctx.drawImage(video, 0, 0, canvas.width, canvas.height);

                    // Draw logo
                    streamoLogoImage.onload = () => { // Ensure logo image is loaded
                        const logoHeight = canvas.height * 0.08; // Logo height as 8% of video height
                        const logoWidth = (streamoLogoImage.width / streamoLogoImage.height) * logoHeight;
                        const padding = canvas.width * 0.02; // Padding as 2% of video width
                        ctx.drawImage(streamoLogoImage, canvas.width - logoWidth - padding, canvas.height - logoHeight - padding, logoWidth, logoHeight);
                        thumbnailPreviewEl.src = canvas.toDataURL('image/jpeg'); // Use JPEG for smaller size
                    };
                    // If logo is already loaded (e.g. from cache or previous draw)
                    if (streamoLogoImage.complete && streamoLogoImage.naturalHeight !== 0) {
                        streamoLogoImage.onload(); // Call onload manually
                    }
                };
                video.onerror = () => {
                    console.error("Error loading video for thumbnail generation.");
                    thumbnailPreviewEl.src = thumbnailPreviewEl.dataset.defaultSrc; // Reset on error
                    if (videoWatermarkInfo) videoWatermarkInfo.classList.add('hidden');
                };
                video.src = e.target.result;
            };
            reader.onerror = () => {
                 console.error("Error reading video file.");
                 thumbnailPreviewEl.src = thumbnailPreviewEl.dataset.defaultSrc; // Reset on error
                 if (videoWatermarkInfo) videoWatermarkInfo.classList.add('hidden');
            };
            reader.readAsDataURL(videoFile);
        }


        function adjustTikTokSectionHeight() {
            if (tiktokVideosSection) {
                const headerHeight = document.querySelector('header')?.offsetHeight || 68;
                const footerHeight = document.querySelector('footer')?.offsetHeight || 60;
                tiktokVideosSection.style.height = `calc(100vh - ${headerHeight}px - ${footerHeight}px)`;
            }
        }

        function initializeTikTokPlayer() {
            if (!tiktokVideosSection) return;
            const observerOptions = { root: tiktokVideosSection, threshold: 0.8 };
            const videoObserver = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    const video = entry.target.querySelector('.tiktok-video-player') || entry.target;
                    if(!video) return; 
                    if (entry.isIntersecting) {
                        if (currentPlayingTikTokVideo && currentPlayingTikTokVideo !== video) {
                            currentPlayingTikTokVideo.pause();
                        }
                        video.play().catch(error => console.log("Video autoplay prevented:", error));
                        currentPlayingTikTokVideo = video;
                    } else {
                        video.pause();
                        if (currentPlayingTikTokVideo === video) {
                            currentPlayingTikTokVideo = null;
                        }
                    }
                });
            }, observerOptions);

            document.querySelectorAll('.tiktok-video-container').forEach(container => {
                videoObserver.observe(container);
                const video = container.querySelector('.tiktok-video-player');
                if (video) {
                    container.addEventListener('click', (event) => {
                        if (event.target.closest('.tiktok-video-actions') || event.target.closest('.tiktok-video-info a')) {
                            return;
                        }
                        if (video.paused) {
                            video.play().catch(error => console.log("Video play prevented:", error));
                        } else {
                            video.pause();
                        }
                    });
                }
            });
        }
        
        async function startCameraStream() {
            if (localStream || !liveVideoPreview) { return; }
            try {
                if(liveStudioError) liveStudioError.classList.add('hidden');
                const constraints = { 
                    video: { facingMode: "user", width: { ideal: 1280 }, height: { ideal: 720 } }, 
                    audio: !isMicMuted 
                };
                localStream = await navigator.mediaDevices.getUserMedia(constraints);
                liveVideoPreview.srcObject = localStream;
                liveVideoPreview.play(); 

                if(liveStatusIndicator) liveStatusIndicator.textContent = 'Live Preview';
                if(liveStatusIndicator) liveStatusIndicator.classList.remove('offline');
                if(liveStatusIndicator) liveStatusIndicator.classList.add('live');
                
                if(startStreamButton) startStreamButton.classList.add('hidden');
                if(stopStreamButton) stopStreamButton.classList.remove('hidden');
                if(liveViewersCount) liveViewersCount.classList.remove('hidden'); 
                
                const audioTracks = localStream.getAudioTracks();
                if (audioTracks.length > 0) {
                    isMicMuted = !audioTracks[0].enabled; 
                    updateMicButton();
                } else { 
                    isMicMuted = true; 
                    if(micIcon) micIcon.textContent = 'mic_off';
                    if(micText) micText.textContent = 'No Mic';
                    if(micToggleButton) micToggleButton.disabled = true; 
                }
                console.log("Camera stream started for Live Studio.");
            } catch (err) {
                console.error("Error accessing media devices.", err);
                if(liveStudioError) {
                    liveStudioError.textContent = `Error: ${err.name} - ${err.message}. Please ensure camera/mic permissions are granted.`;
                    liveStudioError.classList.remove('hidden');
                }
                if(liveStatusIndicator) liveStatusIndicator.textContent = 'Error';
            }
        }

        function stopCameraStreamInternal() { 
            if (localStream) { localStream.getTracks().forEach(track => track.stop()); }
            localStream = null;
            if(liveVideoPreview) liveVideoPreview.srcObject = null;
            if(liveStatusIndicator) liveStatusIndicator.textContent = 'Offline';
            if(liveStatusIndicator) liveStatusIndicator.classList.remove('live');
            if(liveStatusIndicator) liveStatusIndicator.classList.add('offline');
            if(startStreamButton) startStreamButton.classList.remove('hidden');
            if(stopStreamButton) stopStreamButton.classList.add('hidden');
            if(liveViewersCount) liveViewersCount.classList.add('hidden');
            if(liveStudioError) liveStudioError.classList.add('hidden');
            if(micToggleButton) micToggleButton.disabled = false; 
            isMicMuted = false; 
            updateMicButton(); 
            console.log("Camera stream stopped for Live Studio.");
        }
        
        function toggleMicrophone() {
            if (!localStream || localStream.getAudioTracks().length === 0) {
                console.log("No audio track to toggle.");
                 if(liveStudioError) {
                    liveStudioError.textContent = "No microphone detected or permission denied.";
                    liveStudioError.classList.remove('hidden');
                 }
                return;
            }
            const audioTrack = localStream.getAudioTracks()[0];
            audioTrack.enabled = !audioTrack.enabled;
            isMicMuted = !audioTrack.enabled;
            updateMicButton();
        }

        function updateMicButton() {
            if(!micIcon || !micText) return; 
            if (isMicMuted) {
                micIcon.textContent = 'mic_off';
                micText.textContent = 'Unmute';
            } else {
                micIcon.textContent = 'mic';
                micText.textContent = 'Mute';
            }
        }

        // --- Event Listeners ---
        navLinks.forEach(link => {
            link.addEventListener('click', (e) => {
                e.preventDefault();
                const targetSectionId = link.dataset.section;
                if (targetSectionId) setActiveSection(targetSectionId);
            });
        });

        if (mobileMenuOpenButton) mobileMenuOpenButton.addEventListener('click', openMobileMenu);
        if (mobileMenuCloseButton) mobileMenuCloseButton.addEventListener('click', closeMobileMenu);
        mobileMenuLinks.forEach(link => {
            link.addEventListener('click', () => { if (!link.dataset.section) closeMobileMenu(); });
        });

        if (userDropdownButton) userDropdownButton.addEventListener('click', toggleUserDropdown);
        document.addEventListener('click', (event) => {
            if (dropdownMenu && !dropdownMenu.classList.contains('hidden') && 
                userDropdownButton && !userDropdownButton.contains(event.target) && 
                !dropdownMenu.contains(event.target)) {
                toggleUserDropdown();
            }
        });
        profileTabChangers.forEach(button => {
            button.addEventListener('click', () => {
                const section = button.dataset.section;
                const tab = button.dataset.tab;
                if (section && tab) {
                    setActiveSection(section, tab);
                    if (dropdownMenu) dropdownMenu.classList.add('hidden'); 
                }
            });
        });

        if (signinButtonDesktop) signinButtonDesktop.addEventListener('click', () => openModal(signinModal));
        if (signupButtonDesktop) signupButtonDesktop.addEventListener('click', () => openModal(signupModal));
        if (signinButtonMobile) signinButtonMobile.addEventListener('click', () => { openModal(signinModal); closeMobileMenu(); });
        if (signupButtonMobile) signupButtonMobile.addEventListener('click', () => { openModal(signupModal); closeMobileMenu(); });
        if (uploadButton) uploadButton.addEventListener('click', () => openModal(uploadModal));
        if (uploadButtonMobile) uploadButtonMobile.addEventListener('click', () => { openModal(uploadModal); closeMobileMenu(); });

        modalCloseButtons.forEach(button => {
            button.addEventListener('click', () => {
                const modalId = button.dataset.modalClose || button.closest('.modal-overlay')?.id;
                if (modalId) closeModal(document.getElementById(modalId));
                else closeAllModals();
            });
        });
        modals.forEach(modal => {
            if (modal) {
                modal.addEventListener('click', function(event) {
                    if (event.target === this) closeModal(this);
                });
            }
        });

        const logoutLink = document.getElementById('logout-link');
        const logoutLinkMobile = document.getElementById('logout-link-mobile');
        function handleLogout() {
            isLoggedIn = false;
            updateAuthStateUI();
            setActiveSection('tiktok-videos-section'); 
            if (dropdownMenu) dropdownMenu.classList.add('hidden'); 
            closeMobileMenu();
            stopCurrentCameraStream(); 
        }
        if (logoutLink) logoutLink.addEventListener('click', handleLogout);
        if (logoutLinkMobile) logoutLinkMobile.addEventListener('click', handleLogout);
        
        const signinForm = document.getElementById('signin-form');
        if (signinForm) {
            signinForm.addEventListener('submit', (e) => {
                e.preventDefault();
                const email = document.getElementById('signin-email').value;
                const password = document.getElementById('signin-password').value;
                const signinError = document.getElementById('signin-error');
                const signinErrorMsg = signinError ? signinError.querySelector('span:last-child') : null;

                if (email === "user@example.com" && password === "password123") { 
                    isLoggedIn = true;
                    currentUser = { 
                        name: "Demo User", email: "user@example.com", avatar: "https://placehold.co/40x40/10b981/ffffff?text=D",
                        isVerified: false, bio: "Just a demo user exploring Streamo!", followers: "100", following: "50", uploads: "5", walletBalance: "$50.00"
                    };
                    updateAuthStateUI(); closeModal(signinModal); setActiveSection('profile-section', 'my-videos'); 
                    if(signinError) signinError.classList.add('hidden');
                } else {
                    if(signinError && signinErrorMsg) { signinErrorMsg.textContent = "Invalid email or password."; signinError.classList.remove('hidden'); }
                }
            });
        }
        const signupForm = document.getElementById('signup-form');
        if (signupForm) {
            signupForm.addEventListener('submit', (e) => {
                e.preventDefault();
                 const signupError = document.getElementById('signup-error');
                 const signupErrorMsg = signupError ? signupError.querySelector('span:last-child') : null;
                isLoggedIn = true; 
                 currentUser = { 
                    name: document.getElementById('signup-username').value || "New User", email: document.getElementById('signup-email').value,
                    avatar: "https://placehold.co/40x40/3b82f6/ffffff?text=N", isVerified: false, bio: "Excited to be on Streamo!",
                    followers: "0", following: "0", uploads: "0", walletBalance: "$0.00"
                };
                updateAuthStateUI(); closeModal(signupModal); setActiveSection('profile-section', 'settings'); 
                if(signupError) signupError.classList.add('hidden');
            });
        }
        
        function handleSocialLogin(provider) {
            console.log(`Attempting ${provider} login...`);
            isLoggedIn = true;
            currentUser = {
                name: `${provider} User`, email: `${provider.toLowerCase()}user@example.com`,
                avatar: `https://placehold.co/40x40/${Math.floor(Math.random()*16777215).toString(16)}/ffffff?text=${provider.charAt(0)}`,
                isVerified: Math.random() > 0.7, bio: `Logged in with ${provider}!`, followers: Math.floor(Math.random() * 1000).toString(),
                following: Math.floor(Math.random() * 200).toString(), uploads: Math.floor(Math.random() * 20).toString(), walletBalance: `$${(Math.random() * 200).toFixed(2)}`
            };
            updateAuthStateUI(); closeAllModals(); setActiveSection('profile-section', 'my-videos');
            console.log(`${provider} login successful (simulated).`);
        }

        if(googleSigninButton) googleSigninButton.addEventListener('click', () => handleSocialLogin('Google'));
        if(facebookSigninButton) facebookSigninButton.addEventListener('click', () => handleSocialLogin('Facebook'));
        if(tiktokSigninButton) tiktokSigninButton.addEventListener('click', () => handleSocialLogin('TikTok'));
        if(xSigninButton) xSigninButton.addEventListener('click', () => handleSocialLogin('X'));
        if(googleSignupButton) googleSignupButton.addEventListener('click', () => handleSocialLogin('Google'));
        if(facebookSignupButton) facebookSignupButton.addEventListener('click', () => handleSocialLogin('Facebook'));
        if(tiktokSignupButton) tiktokSignupButton.addEventListener('click', () => handleSocialLogin('TikTok'));
        if(xSignupButton) xSignupButton.addEventListener('click', () => handleSocialLogin('X'));

        const uploadForm = document.getElementById('upload-form');
        if (uploadForm) {
            uploadForm.addEventListener('submit', (e) => {
                e.preventDefault();
                const uploadError = document.getElementById('upload-error');
                const uploadErrorMsg = uploadError ? uploadError.querySelector('span:last-child') : null;
                const uploadSuccess = document.getElementById('upload-success');
                const uploadSuccessMsg = uploadSuccess ? uploadSuccess.querySelector('span:last-child') : null;
                const submitButton = uploadForm.querySelector('.form-button-submit');
                
                if(!submitButton) return;
                submitButton.classList.add('submitting'); submitButton.disabled = true;
                const originalButtonText = submitButton.innerHTML;
                submitButton.innerHTML = `<svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path></svg> Uploading...`;

                setTimeout(() => {
                    if(uploadError) uploadError.classList.add('hidden');
                    if(uploadSuccess) uploadSuccess.classList.add('hidden');
                    if(uploadSuccess && uploadSuccessMsg) {
                        uploadSuccessMsg.textContent = "Content uploaded successfully!";
                        uploadSuccess.classList.remove('hidden');
                    }
                    uploadForm.reset(); 
                    if (uploadThumbnailPreview && uploadThumbnailPreview.dataset.defaultSrc) {
                        uploadThumbnailPreview.src = uploadThumbnailPreview.dataset.defaultSrc;
                    }
                    if(videoWatermarkInfo) videoWatermarkInfo.classList.add('hidden'); // Hide watermark info after successful upload
                    
                    submitButton.classList.remove('submitting'); submitButton.disabled = false; submitButton.innerHTML = originalButtonText;
                    setTimeout(() => { closeModal(uploadModal); if(uploadSuccess) uploadSuccess.classList.add('hidden'); }, 2000);
                }, 2500); 
            });
        }
        
        // Event listener for the main file input to generate watermarked video thumbnail
        if (uploadFileInput && uploadThumbnailPreview) {
            uploadFileInput.addEventListener('change', function(event) {
                const file = event.target.files[0];
                const uploadType = document.getElementById('upload-type').value;
                if (file && (uploadType === 'video' || uploadType === 'movie')) {
                    generateWatermarkedVideoThumbnail(file, uploadThumbnailPreview);
                } else if (file && file.type.startsWith('image/')) { // If it's an image (e.g. for music thumbnail)
                     if (videoWatermarkInfo) videoWatermarkInfo.classList.add('hidden');
                     const reader = new FileReader();
                     reader.onload = (e) => uploadThumbnailPreview.src = e.target.result;
                     reader.readAsDataURL(file);
                } else { // Reset if not a video or image
                    uploadThumbnailPreview.src = uploadThumbnailPreview.dataset.defaultSrc;
                    if (videoWatermarkInfo) videoWatermarkInfo.classList.add('hidden');
                }
            });
        }

        // Event listener for the custom thumbnail input
        if (uploadThumbnailInput && uploadThumbnailPreview) {
             setupImagePreview(uploadThumbnailInput, uploadThumbnailPreview); // Use generic image preview for this
             uploadThumbnailInput.addEventListener('change', function(event){
                const file = event.target.files[0];
                // If a custom thumbnail is chosen, it overrides the video-generated one.
                // Hide watermark info if custom thumbnail is an image, as it won't be watermarked here.
                if (file && file.type.startsWith('image/')) {
                     if (videoWatermarkInfo) videoWatermarkInfo.classList.add('hidden');
                } else if (uploadFileInput.files[0] && (document.getElementById('upload-type').value === 'video' || document.getElementById('upload-type').value === 'movie')) {
                    // If custom thumbnail is cleared or not an image, and a video is selected in main input, re-show watermark info
                     if (videoWatermarkInfo) videoWatermarkInfo.classList.remove('hidden');
                }
             });
        }


        profileTabs.forEach(tab => {
            tab.addEventListener('click', () => setActiveProfileTab(tab.dataset.profileTab));
        });
        
        setupImagePreview(profileAvatarInput, profileAvatarPreview);
        setupImagePreview(profileBannerInput, profileBannerPreview);
        // Initial setup for upload thumbnail preview's default src
        if (uploadThumbnailPreview && !uploadThumbnailPreview.dataset.defaultSrc) {
            uploadThumbnailPreview.dataset.defaultSrc = uploadThumbnailPreview.src;
        }


        if (startStreamButton) startStreamButton.addEventListener('click', startCameraStream);
        if (stopStreamButton) stopStreamButton.addEventListener('click', stopCameraStreamInternal);
        if (micToggleButton) micToggleButton.addEventListener('click', toggleMicrophone);
        
        window.addEventListener('resize', adjustTikTokSectionHeight);
        adjustTikTokSectionHeight(); 

        window.addEventListener('beforeunload', () => {
            const liveStudioSec = document.getElementById('live-studio-section');
            if (localStream && liveStudioSec && !liveStudioSec.classList.contains('hidden')) {
                stopCurrentCameraStream();
            }
        });

        const initialActiveSection = document.querySelector('.nav-link.active')?.dataset.section || 'tiktok-videos-section';
        setActiveSection(initialActiveSection);

    });
</script>
</body>
</html>
