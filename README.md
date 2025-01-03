Burada yaptığım gezi ve tatil planlayıcısı(Trivago,AirBnb) dataBase paylaştım. Şemada toplam 21 tablo vardır ve hepsi birbiriyle bağşantılıdır.
users
hotels
rooms
reservations
reviews
payment_methods
transactions
amenities
hotel_amenities
membership_packages
favorites
cancellations
photos
cities
countries
activities
coupons
car_rentals
hotel_management
flights
flight_reservations
-- SQL Tablolarının Sırasıyla Oluşturulması (Hatasız)

CREATE TABLE countries (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    continent VARCHAR(100)
);

CREATE TABLE cities (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    country_id INT,
    FOREIGN KEY (country_id) REFERENCES countries(id)
);

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    phone_number VARCHAR(20),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE hotels (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    description TEXT,
    address VARCHAR(255),
    city_id INT,
    star_rating INT,
    contact_email VARCHAR(100),
    contact_phone VARCHAR(20),
    FOREIGN KEY (city_id) REFERENCES cities(id)
);

CREATE TABLE rooms (
    id INT AUTO_INCREMENT PRIMARY KEY,
    hotel_id INT,
    room_type VARCHAR(50),
    price_per_night DECIMAL(10, 2),
    availability BOOLEAN DEFAULT TRUE,
    FOREIGN KEY (hotel_id) REFERENCES hotels(id)
);

CREATE TABLE reservations (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    room_id INT,
    check_in_date DATE,
    check_out_date DATE,
    total_price DECIMAL(10, 2),
    status VARCHAR(50),
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (room_id) REFERENCES rooms(id)
);

CREATE TABLE reviews (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    hotel_id INT,
    rating INT NOT NULL,
    comment TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (hotel_id) REFERENCES hotels(id)
);

CREATE TABLE payment_methods (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    card_number VARCHAR(20),
    cardholder_name VARCHAR(100),
    expiration_date DATE,
    cvv VARCHAR(5),
    FOREIGN KEY (user_id) REFERENCES users(id)
);

CREATE TABLE transactions (
    id INT AUTO_INCREMENT PRIMARY KEY,
    reservation_id INT,
    payment_method_id INT,
    amount DECIMAL(10, 2),
    transaction_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (reservation_id) REFERENCES reservations(id),
    FOREIGN KEY (payment_method_id) REFERENCES payment_methods(id)
);

CREATE TABLE amenities (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

CREATE TABLE hotel_amenities (
    id INT AUTO_INCREMENT PRIMARY KEY,
    hotel_id INT,
    amenity_id INT,
    FOREIGN KEY (hotel_id) REFERENCES hotels(id),
    FOREIGN KEY (amenity_id) REFERENCES amenities(id)
);

CREATE TABLE flights (
    id INT AUTO_INCREMENT PRIMARY KEY,
    flight_number VARCHAR(50) NOT NULL,
    departure_city_id INT,
    arrival_city_id INT,
    departure_time DATETIME,
    arrival_time DATETIME,
    price DECIMAL(10, 2),
    availability BOOLEAN DEFAULT TRUE,
    FOREIGN KEY (departure_city_id) REFERENCES cities(id),
    FOREIGN KEY (arrival_city_id) REFERENCES cities(id)
);

CREATE TABLE flight_bookings (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    flight_id INT,
    booking_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    total_price DECIMAL(10, 2),
    status VARCHAR(50),
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (flight_id) REFERENCES flights(id)
);

CREATE TABLE membership_packages (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    price DECIMAL(10, 2),
    features JSON,
    duration_days INT
);

CREATE TABLE favorites (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    hotel_id INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (hotel_id) REFERENCES hotels(id)
);

CREATE TABLE cancellations (
    id INT AUTO_INCREMENT PRIMARY KEY,
    reservation_id INT,
    cancellation_reason TEXT,
    cancellation_date DATE,
    refund_status VARCHAR(50),
    FOREIGN KEY (reservation_id) REFERENCES reservations(id)
);

CREATE TABLE photos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    reference_id INT,
    type VARCHAR(50),
    url VARCHAR(255),
    uploaded_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE activities (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    description TEXT,
    price DECIMAL(10, 2),
    destination_id INT,
    FOREIGN KEY (destination_id) REFERENCES cities(id)
);

CREATE TABLE coupons (
    id INT AUTO_INCREMENT PRIMARY KEY,
    code VARCHAR(50),
    discount_percentage DECIMAL(5, 2),
    valid_until DATE,
    used_count INT DEFAULT 0
);

CREATE TABLE car_rentals (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    price_per_day DECIMAL(10, 2),
    available BOOLEAN DEFAULT TRUE,
    location_id INT,
    FOREIGN KEY (location_id) REFERENCES cities(id)
);

CREATE TABLE hotel_management (
    id INT AUTO_INCREMENT PRIMARY KEY,
    hotel_id INT,
    manager_name VARCHAR(100),
    contact_number VARCHAR(20),
    email VARCHAR(100),
    FOREIGN KEY (hotel_id) REFERENCES hotels(id)
);
