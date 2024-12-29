const express = require('express');
const axios = require('axios');

const app = express();
const port = process.env.PORT || 3000;

// Включаем поддержку CORS для запроса с другого домена
const cors = require('cors');
app.use(cors());

// Обработка POST-запроса для получения аватарки по никнейму Roblox
app.get('/get-avatar', async (req, res) => {
    const username = req.query.username;
    
    if (!username) {
        return res.status(400).json({ error: 'Никнейм не указан' });
    }

    try {
        // Запрос к Roblox API для получения информации о пользователе
        const response = await axios.get(`https://api.roblox.com/users/get-by-username?username=${username}`);

        if (response.data.errorMessage) {
            return res.status(404).json({ error: 'Пользователь не найден' });
        }

        const userId = response.data.Id;

        // Получаем аватарку пользователя
        const avatarResponse = await axios.get(`https://thumbnails.roblox.com/v1/users/avatar-headshot?userIds=${userId}&size=150x150&format=Png`);

        if (avatarResponse.data.data.length > 0) {
            const avatarUrl = avatarResponse.data.data[0].imageUrl;
            return res.json({ avatarUrl });
        } else {
            return res.status(404).json({ error: 'Не удалось получить аватарку' });
        }
    } catch (error) {
        console.error(error);
        res.status(500).json({ error: 'Ошибка сервера' });
    }
});

// Запуск сервера
app.listen(port, () => {
    console.log(`Сервер работает на порту ${port}`);
});
