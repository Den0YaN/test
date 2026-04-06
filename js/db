// ============================================
// ПОДКЛЮЧЕНИЕ К SUPABASE
// ============================================

const SUPABASE_URL = 'https://gmcqxgxwtczjlwyifwew.supabase.co';
const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImdtY3F4Z3h3dGN6amx3eWlmd2V3Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzU0MjIxMTAsImV4cCI6MjA5MDk5ODExMH0.cM6xm9qCRbl-c1h-pWOWKSeAozYUy7KpJjua79JgFuk';

const db = window.supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);

// ============================================
// ФУНКЦИИ АВТОРИЗАЦИИ
// ============================================

async function loginUser(login, password) {
    const { data, error } = await db.from('users')
        .select('*')
        .eq('login', login)
        .eq('password', password);

    if (error || !data || data.length === 0) return null;
    return data[0];
}

async function registerUser(login, password, email) {
    const { data: existing } = await db.from('users')
        .select('*')
        .eq('login', login);

    if (existing && existing.length > 0) {
        return { success: false, message: '❌ Пользователь уже существует' };
    }

    const { count } = await db.from('users')
        .select('*', { count: 'exact', head: true });

    const role = count === 0 ? 'admin' : 'user';

    const { error } = await db.from('users').insert([{
        login,
        password,
        email,
        role,
        name: login
    }]);

    if (error) {
        return { success: false, message: '❌ Ошибка регистрации' };
    }

    return {
        success: true,
        message: role === 'admin'
            ? '✅ Вы стали АДМИНИСТРАТОРОМ!'
            : '✅ Регистрация успешна!'
    };
}
